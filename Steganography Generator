import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import os

print('Steganography Generator - Hide & Reveal Messages')
print('Version v1.1.0')
print('Created by Joseph Morrison')
print('Licensed Under CC BY-NC-ND 4.0 License')

def encode_message():
    input_image_path = filedialog.askopenfilename(filetypes=[("PNG Images", "*.png")])
    if not input_image_path:
        return

    message = message_entry.get("1.0", tk.END).strip()
    if not message:
        messagebox.showerror("Error", "No message to hide!")
        return

    image = Image.open(input_image_path)
    binary_message = ''.join(format(ord(i), '08b') for i in message) + '1111111111111110'
    data = iter(image.getdata())
    
    new_pixels = []
    for i in range(0, len(binary_message), 3):
        pixels = [value for value in next(data)]
        for j in range(3):
            if i + j < len(binary_message):
                pixels[j] = pixels[j] & ~1 | int(binary_message[i + j])
        new_pixels.append(tuple(pixels))
    
    for pixel in data:
        new_pixels.append(pixel)

    encoded_image = Image.new(image.mode, image.size)
    encoded_image.putdata(new_pixels)
    
    save_path = filedialog.asksaveasfilename(defaultextension=".png", filetypes=[("PNG Images", "*.png")])
    if save_path:
        encoded_image.save(save_path)
        messagebox.showinfo("Success", "Message encoded and saved successfully.")

def decode_message():
    input_image_path = filedialog.askopenfilename(filetypes=[("PNG Images", "*.png")])
    if not input_image_path:
        return

    image = Image.open(input_image_path)
    binary_data = ""
    for pixel in image.getdata():
        for color in pixel[:3]:
            binary_data += str(color & 1)
    
    all_bytes = [binary_data[i:i+8] for i in range(0, len(binary_data), 8)]
    message = ""
    for byte in all_bytes:
        if byte == '11111110':
            break
        message += chr(int(byte, 2))

    output_text.delete("1.0", tk.END)
    output_text.insert(tk.END, message)

# GUI Setup
root = tk.Tk()
root.title("Steganography Generator - Hide & Reveal Messages")

frame = tk.Frame(root, padx=10, pady=10)
frame.pack()

tk.Label(frame, text="Enter Message to Hide:").grid(row=0, column=0, sticky="w")
message_entry = tk.Text(frame, height=5, width=50)
message_entry.grid(row=1, column=0, columnspan=2)

tk.Button(frame, text="Hide Message in Image", command=encode_message).grid(row=2, column=0, pady=10)
tk.Button(frame, text="Reveal Message from Image", command=decode_message).grid(row=2, column=1, pady=10)

tk.Label(frame, text="Hidden Message Output:").grid(row=3, column=0, sticky="w")
output_text = tk.Text(frame, height=5, width=50)
output_text.grid(row=4, column=0, columnspan=2)

root.mainloop()

