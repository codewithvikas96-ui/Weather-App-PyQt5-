import sys
import requests
from PyQt5.QtWidgets import (QApplication, QWidget, QMainWindow, QLabel,
                             QVBoxLayout,QPushButton, QLineEdit)
from PyQt5.QtCore import Qt

class Window(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Weather App")
        self.weather_UI()

    def weather_UI(self):
        central_widget = QWidget()
        main_layout = QVBoxLayout()

        self.input_text = QLabel("Enter the city name: ",self)
        self.input_box = QLineEdit(self)
        self.weather_btn = QPushButton("Get Weather",self)
        self.temp_label = QLabel(self)
        self.temp_icon = QLabel(self)
        self.temp_desc = QLabel(self)

        main_layout.addWidget(self.input_text)
        main_layout.addWidget(self.input_box)
        main_layout.addWidget(self.weather_btn)
        main_layout.addWidget(self.temp_label)
        main_layout.addWidget(self.temp_icon)
        main_layout.addWidget(self.temp_desc)

        self.input_text.setObjectName("inputtxtOBJ")
        self.input_box.setObjectName("inputboxOBJ")
        self.weather_btn.setObjectName("weatherOBJ")
        self.temp_label.setObjectName("tempLabelOBJ")
        self.temp_icon.setObjectName("tempIconOBJ")
        self.temp_desc.setObjectName("tempDescOBJ")

        self.input_text.setAlignment(Qt.AlignCenter)
        self.input_box.setAlignment(Qt.AlignCenter)
        self.temp_label.setAlignment(Qt.AlignCenter)
        self.temp_icon.setAlignment(Qt.AlignCenter)
        self.temp_desc.setAlignment(Qt.AlignCenter)

        self.setStyleSheet("""
            QWidget {
                background-color: #1E1F28;
            }
            QLabel, QPushButton {
                font-family: 'Times New Roman';
            }
            QLabel#inputtxtOBJ {
                font-size: 40px;
                color: #FFD166;  
            }
            QLineEdit#inputboxOBJ {
                font-size: 40px;
                font-family: Oswald;
                color: #00FFAA;  
                background-color: #2C2F48;  
                border-radius: 10px;
                border: 2px solid #444;
                padding: 5px;
            }
            QPushButton#weatherOBJ {
                font-size: 30px;
                font-weight: bold;
                color: white;
                background: qlineargradient(x1:0, y1:0, x2:1, y2:1,
                                            stop:0 #6B5BFF, stop:1 #8A75FF);
                border-radius: 15px;
                padding: 10px;
            }
            QPushButton#weatherOBJ:hover {
                background: qlineargradient(x1:0, y1:0, x2:1, y2:1,
                                            stop:0 #8A75FF, stop:1 #6B5BFF);
            }
            QLabel#tempLabelOBJ {
                font-size: 75px;
                color: #FFD166;  
            }
            QLabel#tempIconOBJ {
                font-size: 100px;
                font-family: "Segoe UI Emoji";
                color: #FFFAF0;  
            }
            QLabel#tempDescOBJ {
                font-size: 50px;
                color: #4ADE80;  
            }
        """)


        self.weather_btn.clicked.connect(self.get_weather)
        self.input_box.returnPressed.connect(self.get_weather)

        central_widget.setLayout(main_layout)
        self.setCentralWidget(central_widget)


    def get_weather(self):
        api_key = "YOUR_API_KEY"
        city = self.input_box.text().strip()

        if not city:
            self.display_error("Please enter a city name")
            return

        url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"

        try:
            response = requests.get(url)
            response.raise_for_status() 
            data = response.json()

            if data["cod"] == 200:
                self.display_weather(data)

            
        except requests.exceptions.HTTPError as http_error:
            match http_error.response.status_code:
                case 400:
                    self.display_error("Bad request:\nPlease check your input")
                case 401:
                    self.display_error("Unauthorized:\nInvalid API key")
                case 403:
                    self.display_error("Forbidden:\nAccess is denied")
                case 404:
                    self.display_error("Not found:\nCity not found")
                case 500:
                    self.display_error("Internal Server Error:\nPlease try again later")
                case 502:
                    self.display_error("Bad Gateway:\nInvalid response from the server")
                case 503:
                    self.display_error("Service Unavailable:\nServer is down")
                case 504:
                    self.display_error("Gateway Timeout:\nNo response from the server")
                case _:
                    self.display_error(f"HTTP error occurred:\n{http_error}")
        except requests.exceptions.ConnectionError:
            self.display_error("Connection Error:\nCheck your internet connection")
        except requests.exceptions.Timeout:
            self.display_error("Timeout Error:\nThe request timed out")
        except requests.exceptions.TooManyRedirects:
            self.display_error("Too many Redirects:\nCheck the URL")
        except requests.exceptions.RequestException as req_error:
            self.display_error(f"Request Error:\n{req_error}")          

    def display_error(self,message):
        self.temp_label.setStyleSheet("font-size: 30px; color: #FF4C4C;;")
        self.temp_label.setText(message)
        self.temp_icon.setText("")
        self.temp_desc.setText("")

    
    def display_weather(self, data):
        self.temp_label.setStyleSheet("font-size: 75px;")

        temperature_c = data["main"]["temp"]
        weather_id = data["weather"][0]["id"]
        weather_description = data["weather"][0]["description"]

        self.temp_label.setText(f"{temperature_c:.0f}°C")
        self.temp_icon.setText(self.get_weather_emoji(weather_id))
        self.temp_desc.setStyleSheet("font-size: 50px;")
        self.temp_desc.setText(weather_description.title())


    @staticmethod
    def get_weather_emoji(weather_id):

        if 200 <= weather_id <= 232:
            return "⛈"
        elif 300 <= weather_id <= 321:
            return "🌦"
        elif 500 <= weather_id <= 531:
            return "🌧"
        elif 600 <= weather_id <= 622:
            return "❄"
        elif 701 <= weather_id <= 741:
            return "🌫"
        elif weather_id == 762:
            return "🌋"
        elif weather_id == 771:
            return "💨"
        elif weather_id == 781:
            return "🌪"
        elif weather_id == 800:
            return "☀"
        elif 801 <= weather_id <= 804:
            return "☁"
        else:
            return ""


def main():
    app = QApplication(sys.argv)
    my_window = Window()
    my_window.show()
    sys.exit(app.exec_())

if __name__ == '__main__':
    main()
