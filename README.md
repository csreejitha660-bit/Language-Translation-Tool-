# Language-Translation-Tool-
AI Language Translation Tool

import tkinter as tk
from tkinter import ttk, messagebox
from deep_translator import GoogleTranslator

root = tk.Tk()
root.title("Language Translation Tool")
root.geometry("650x450")
root.configure(bg="#f5f5f7")

LANGUAGES = {
    'English': 'en',
    'Spanish': 'es',
    'French': 'fr',
    'German': 'de',
    'Italian': 'it',
    'Hindi': 'hi',
    'Chinese (Simplified)': 'zh-CN',
    'Japanese': 'ja',
    'Arabic': 'ar',
    'Russian': 'ru',
    'Portuguese': 'pt'
}
def translate_text():
    """Fetches input text, processes translation via API, and updates the UI."""
    source_lang_name = src_lang_combo.get()
    target_lang_name = dest_lang_combo.get()
    text_to_translate = input_text.get("1.0", tk.END).strip()

    if not text_to_translate:
        messagebox.showwarning("Empty Input", "Please enter some text to translate.")
        return

    # Map language names to their ISO codes
    src_code = LANGUAGES[source_lang_name]
    dest_code = LANGUAGES[target_lang_name]

    try:
        # Call the translation engine
        translated = GoogleTranslator(source=src_code, target=dest_code).translate(text_to_translate)
        
        # Clear the output box and insert the new translation
        output_text.config(state=tk.NORMAL)
        output_text.delete("1.0", tk.END)
        output_text.insert(tk.END, translated)
        output_text.config(state=tk.DISABLED)
    except Exception as e:
        messagebox.showerror("Translation Error", f"Could not connect to translation service.\nError: {e}")
def copy_to_clipboard():
    """Copies the translated text to the system clipboard."""
    translated_content = output_text.get("1.0", tk.END).strip()
    if translated_content:
        root.clipboard_clear()
        root.clipboard_append(translated_content)
        messagebox.showinfo("Copied", "Translated text copied to clipboard!")
    else:
        messagebox.showwarning("Nothing to Copy", "The translation output is empty.")

header = tk.Label(root, text="Instant Language Translator", font=("Helvetica", 16, "bold"), bg="#f5f5f7", fg="#333333")
header.pack(pady=15)

selector_frame = tk.Frame(root, bg="#f5f5f7")
selector_frame.pack(pady=10)
tk.Label(selector_frame, text="From:", font=("Helvetica", 10), bg="#f5f5f7").grid(row=0, column=0, padx=5)
src_lang_combo = ttk.Combobox(selector_frame, values=list(LANGUAGES.keys()), state="readonly", width=18)
src_lang_combo.grid(row=0, column=1, padx=10)
src_lang_combo.set('English')
tk.Label(selector_frame, text="To:", font=("Helvetica", 10), bg="#f5f5f7").grid(row=0, column=2, padx=5)
dest_lang_combo = ttk.Combobox(selector_frame, values=list(LANGUAGES.keys()), state="readonly", width=18)
dest_lang_combo.grid(row=0, column=3, padx=10)
dest_lang_combo.set('Spanish')

content_frame = tk.Frame(root, bg="#f5f5f7")
content_frame.pack(padx=20, pady=10, fill=tk.BOTH, expand=True)

input_container = tk.Frame(content_frame, bg="#f5f5f7")
input_container.pack(side=tk.LEFT, fill=tk.BOTH, expand=True, padx=5)
tk.Label(input_container, text="Enter Text:", font=("Helvetica", 10, "bold"), bg="#f5f5f7", fg="#555555").pack(anchor=tk.W)
input_text = tk.Text(input_container, height=10, width=30, wrap=tk.WORD, font=("Helvetica", 10))
input_text.pack(fill=tk.BOTH, expand=True, pady=5)

output_container = tk.Frame(content_frame, bg="#f5f5f7")
output_container.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True, padx=5)
tk.Label(output_container, text="Translation:", font=("Helvetica", 10, "bold"), bg="#f5f5f7", fg="#555555").pack(anchor=tk.W)
output_text = tk.Text(output_container, height=10, width=30, wrap=tk.WORD, font=("Helvetica", 10), state=tk.DISABLED)
output_text.pack(fill=tk.BOTH, expand=True, pady=5)

button_frame = tk.Frame(root, bg="#f5f5f7")
button_frame.pack(pady=20)
translate_btn = tk.Button(button_frame, text="Translate", command=translate_text, bg="#007aff", fg="white", font=("Helvetica", 11, "bold"), padx=15, pady=5, relief=tk.FLAT)
translate_btn.grid(row=0, column=0, padx=10)
copy_btn = tk.Button(button_frame, text="Copy Result", command=copy_to_clipboard, bg="#34c759", fg="white", font=("Helvetica", 11), padx=15, pady=5, relief=tk.FLAT)
copy_btn.grid(row=0, column=1, padx=10)

# Run the app loop
root.mainloop()
