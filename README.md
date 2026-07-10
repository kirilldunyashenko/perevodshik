# perevodshik
цель: создание приложения для быстрого перевода текста между тремя языками(ru,en,zh)
import customtkinter as ctk
from deep_translator import GoogleTranslator


ctk.set_appearance_mode("System")
ctk.set_default_color_theme("blue")

class TranslatorApp(ctk.CTk):
    def __init__(self):
        super().__init__()


        self.title("Переводчик ")
        self.geometry("500x480")
        self.resizable(False, False)


        self.languages = {
            "Русский": "ru",
            "Английский": "en",
            "Китайский": "zh-CN"
        }


        self.nav_frame = ctk.CTkFrame(self, fg_color="transparent")
        self.nav_frame.pack(pady=15, padx=20, fill="x")


        self.combo_from = ctk.CTkComboBox(self.nav_frame, values=list(self.languages.keys()), width=140)
        self.combo_from.pack(side="left")
        self.combo_from.set("Английский")


        self.btn_swap = ctk.CTkButton(self.nav_frame, text="⇄", width=40, font=("Arial", 16, "bold"),
                                      command=self.swap_languages)
        self.btn_swap.pack(side="left", padx=15)


        self.combo_to = ctk.CTkComboBox(self.nav_frame, values=list(self.languages.keys()), width=140)
        self.combo_to.pack(side="left")
        self.combo_to.set("Русский")


        self.label_input = ctk.CTkLabel(self, text="Введите текст:", font=("Arial", 12, "bold"))
        self.label_input.pack(anchor="w", padx=20, pady=(10, 2))

        self.text_input = ctk.CTkTextbox(self, height=120, activate_scrollbars=True)
        self.text_input.pack(fill="x", padx=20)


        self.btn_translate = ctk.CTkButton(self, text="Перевести", font=("Arial", 14, "bold"), height=40,
                                           command=self.translate_text)
        self.btn_translate.pack(fill="x", padx=20, pady=15)


        self.label_output = ctk.CTkLabel(self, text="Перевод:", font=("Arial", 12, "bold"))
        self.label_output.pack(anchor="w", padx=20, pady=(0, 2))

        self.text_output = ctk.CTkTextbox(self, height=120, activate_scrollbars=True)
        self.text_output.pack(fill="x", padx=20)

    def translate_text(self):

        text_to_translate = self.text_input.get("1.0", "end-1c").strip()

        if not text_to_translate:
            self.text_output.delete("1.0", "end")
            self.text_output.insert("1.0", "Пожалуйста, введите текст для перевода...")
            return


        lang_from_name = self.combo_from.get()
        lang_to_name = self.combo_to.get()

        lang_from_code = self.languages[lang_from_name]
        lang_to_code = self.languages[lang_to_name]

        try:

            translated = GoogleTranslator(source=lang_from_code, target=lang_to_code).translate(text_to_translate)


            self.text_output.delete("1.0", "end")
            self.text_output.insert("1.0", translated)
        except Exception as e:
            self.text_output.delete("1.0", "end")
            self.text_output.insert("1.0", f"Ошибка перевода: Проверьте интернет-соединение.")

    def swap_languages(self):

        current_from = self.combo_from.get()
        current_to = self.combo_to.get()


        self.combo_from.set(current_to)
        self.combo_to.set(current_from)


        input_text = self.text_input.get("1.0", "end-1c").strip()
        output_text = self.text_output.get("1.0", "end-1c").strip()


        if output_text and not output_text.startswith("Ошибка") and not output_text.startswith("Пожалуйста"):
            self.text_input.delete("1.0", "end")
            self.text_input.insert("1.0", output_text)

            self.text_output.delete("1.0", "end")
            self.text_input.focus()


if __name__ == "__main__":
    app = TranslatorApp()
    app.mainloop()
