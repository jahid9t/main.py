from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.uix.scrollview import ScrollView
from kivy.uix.anchorlayout import AnchorLayout
from kivy.uix.relativelayout import RelativeLayout
from kivy.uix.togglebutton import ToggleButton
from kivy.core.window import Window

Window.clearcolor = (0, 0, 0, 1)  # Black background

class ShadowApp(App):
    def build(self):
        self.title = "Shadow"

        main_layout = BoxLayout(orientation='vertical', padding=10, spacing=10)

        # Top layout: Settings button and Title
        top_layout = RelativeLayout(size_hint=(1, None), height=40)

        # Settings toggle button (top-left)
        self.settings_button = ToggleButton(
            text='Settings: OFF',
            size_hint=(None, None),
            size=(120, 40),
            pos_hint={'x': 0, 'center_y': 0.5},
            bold=True
        )
        self.settings_button.bind(on_press=self.toggle_settings)
        top_layout.add_widget(self.settings_button)

        # Title (center)
        title_label = Label(
            text="Welcome to Shadow",
            size_hint=(None, None),
            size=(300, 40),
            pos_hint={'center_x': 0.5, 'center_y': 0.5},
            bold=True,
            font_size='20sp',
            color=(1, 1, 1, 1)
        )
        top_layout.add_widget(title_label)

        main_layout.add_widget(top_layout)

        # Typing preview in center
        self.typing_preview = Label(
            text="",
            size_hint=(1, None),
            height=20,
            font_size='14sp',
            halign='center',
            color=(1, 1, 1, 1)
        )
        main_layout.add_widget(self.typing_preview)

        # Scrollable chat area
        scroll_view = ScrollView(size_hint=(1, 1))
        self.chat_layout = BoxLayout(orientation='vertical', size_hint_y=None, spacing=5)
        self.chat_layout.bind(minimum_height=self.chat_layout.setter('height'))
        scroll_view.add_widget(self.chat_layout)
        main_layout.add_widget(scroll_view)

        # Bottom layout for input and buttons
        bottom_layout = BoxLayout(size_hint=(1, None), height=50, spacing=10)

        self.text_input = TextInput(
            hint_text="Type a message...",
            multiline=False,
            foreground_color=(1, 1, 1, 1),
            background_color=(0.2, 0.2, 0.2, 1)
        )
        self.text_input.bind(text=self.show_typing)

        send_button = Button(text="Send", size_hint=(None, 1), width=80)
        send_button.bind(on_press=self.send_message)

        photo_button = Button(text="📷", size_hint=(None, 1), width=50)

        bottom_layout.add_widget(self.text_input)
        bottom_layout.add_widget(send_button)
        bottom_layout.add_widget(photo_button)

        main_layout.add_widget(bottom_layout)  # সংশোধন করা হয়েছে

        return main_layout

    def toggle_settings(self, instance):
        if instance.state == 'down':
            instance.text = "Settings: ON"
        else:
            instance.text = "Settings: OFF"

    def show_typing(self, instance, value):
        self.typing_preview.text = f"Typing: {value}" if value else ""

    def send_message(self, instance):
        text = self.text_input.text.strip()
        if text:
            message_box = AnchorLayout(anchor_x='right', size_hint_y=None, height=30)
            message_label = Label(
                text=f"You: {text}",
                size_hint=(None, None),
                color=(1, 1, 1, 1)
            )
            message_label.bind(texture_size=message_label.setter('size'))
            message_box.add_widget(message_label)
            self.chat_layout.add_widget(message_box)

            self.text_input.text = ""
            self.typing_preview.text = ""

if __name__ == "__main__":
    ShadowApp().run()
    # updated
    
