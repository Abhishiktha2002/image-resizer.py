# image-resizer.py
import os
from PIL import Image

# === CONFIGURATION ===
input_folder = "input_images"
output_folder = "output_images"
resize_to = (800, 600)  # Width x Height
output_format = "JPEG"  # Can be "JPEG", "PNG", etc.
file_extensions = (".jpg", ".jpeg", ".png", ".bmp", ".gif")  # Accepted formats

# === ENSURE OUTPUT FOLDER EXISTS ===
os.makedirs(output_folder, exist_ok=True)

# === RESIZE & CONVERT ===
for filename in os.listdir(input_folder):
    if filename.lower().endswith(file_extensions):
        img_path = os.path.join(input_folder, filename)
        try:
            with Image.open(img_path) as img:
                img = img.convert("RGB")  # Ensures compatibility with JPEG
                img_resized = img.resize(resize_to, Image.ANTIALIAS)

                base_name = os.path.splitext(filename)[0]
                output_path = os.path.join(output_folder, f"{base_name}.{output_format.lower()}")

                img_resized.save(output_path, output_format)
                print(f"Processed: {filename} → {output_path}")
        except Exception as e:
            print(f"Error processing {filename}: {e}")

print("✅ Batch image resizing completed.")
