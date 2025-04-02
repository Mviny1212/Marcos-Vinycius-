from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.label import Label

class AdventureGame(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(orientation='vertical', **kwargs)
        self.inventory = []
        self.story_label = Label(text="Você está em uma floresta escura. Escolha um caminho.")
        self.inventory_label = Label(text="Inventário: Vazio")
        self.add_widget(self.story_label)
        self.add_widget(self.inventory_label)
        self.button1 = Button(text="Esquerda")
        self.button2 = Button(text="Direita")
        self.button1.bind(on_press=lambda x: self.choose_path("Esquerda"))
        self.button2.bind(on_press=lambda x: self.choose_path("Direita"))
        self.add_widget(self.button1)
        self.add_widget(self.button2)
    
    def choose_path(self, choice):
        if choice == "Esquerda":
            self.story_label.text = "Você encontrou um rio. O que fazer?"
            self.update_buttons("Nadar", "Construir Jangada")
        elif choice == "Direita":
            self.story_label.text = "Um urso feroz apareceu! O que fazer?"
            self.update_buttons("Lutar", "Correr")
        elif choice == "Nadar":
            self.story_label.text = "A correnteza era forte. Você se afogou! Fim de jogo."
            self.update_buttons("Reiniciar", "")
        elif choice == "Construir Jangada":
            self.story_label.text = "Você construiu uma jangada e atravessou com sucesso! Você encontrou uma espada."
            self.inventory.append("Espada")
            self.update_inventory()
            self.update_buttons("Seguir em frente", "")
        elif choice == "Lutar":
            if "Espada" in self.inventory:
                self.story_label.text = "Você derrotou o urso com sua espada! Você venceu!"
                self.update_buttons("Reiniciar", "")
            else:
                self.story_label.text = "O urso era muito forte. Você perdeu!"
                self.update_buttons("Reiniciar", "")
        elif choice == "Correr":
            self.story_label.text = "Você escapou e encontrou uma cabana segura. Você venceu!"
            self.update_buttons("Reiniciar", "")
        elif choice == "Seguir em frente":
            self.story_label.text = "Você encontrou um baú misterioso. Você venceu!"
            self.update_buttons("Reiniciar", "")
        elif choice == "Reiniciar":
            self.story_label.text = "Você está em uma floresta escura. Escolha um caminho."
            self.inventory.clear()
            self.update_inventory()
            self.update_buttons("Esquerda", "Direita")
    
    def update_buttons(self, text1, text2):
        self.button1.text = text1
        self.button2.text = text2 if text2 else ""
        self.button2.disabled = not bool(text2)
    
    def update_inventory(self):
        self.inventory_label.text = f"Inventário: {', '.join(self.inventory) if self.inventory else 'Vazio'}"

class AdventureApp(App):
    def build(self):
        return AdventureGame()

if __name__ == '__main__':
    AdventureApp().run()
