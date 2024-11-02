import qrcode
from tkinter import Tk, Label, Entry, Button, Canvas, filedialog, messagebox
from PIL import Image, ImageTk

def generate_qrcode():
    # Get URL from the entry field
    url = entry.get()
    
    # Create QR code object
    global qr_image  # Define as global to use in save_qrcode function
    qr = qrcode.QRCode(
        version=1,
        error_correction=qrcode.constants.ERROR_CORRECT_L,
        box_size=10,
        border=4,
    )
    
    # Add URL data and generate the QR code image
    qr.add_data(url)
    qr.make(fit=True)
    qr_image = qr.make_image(fill_color="black", back_color="white")
    
    # Convert the QR code image for tkinter display
    img_resized = qr_image.resize((200, 200))  # Resize for better display in the GUI
    img_tk = ImageTk.PhotoImage(img_resized)
    
    # Update the canvas with the new QR code image
    canvas.create_image(0, 0, anchor="nw", image=img_tk)
    canvas.image = img_tk  # Store reference to prevent garbage collection

def save_qrcode():
    if qr_image:
        # Prompt the user to choose a save location and file name
        file_path = filedialog.asksaveasfilename(defaultextension=".png", filetypes=[("PNG files", "*.png")])
        if file_path:
            qr_image.save(file_path)
            messagebox.showinfo("QR Code Generator", f"QR code saved at {file_path}")
    else:
        messagebox.showerror("QR Code Generator", "Please generate a QR code first.")

# Create main window
root = Tk()
root.title("QR Code Generator")
root.geometry("400x450")

# Label for URL entry
Label(root, text="Enter URL:").pack(pady=10)

# Entry field for URL input
entry = Entry(root, width=40)
entry.pack(pady=5)

# Button to generate QR code
Button(root, text="Generate QR Code", command=generate_qrcode).pack(pady=10)

# Canvas to display the QR code
canvas = Canvas(root, width=200, height=200)
canvas.pack()

# Button to save QR code
Button(root, text="Save QR Code", command=save_qrcode).pack(pady=10)

# Initialize global variable for the QR code image
qr_image = None

# Run the main event loop
root.mainloop()
