# Snake Game Python Repo
_
<p>This Repo is for snake game</p>
This is project Brick Breaker
_


##Code


import sys
import numpy as np

from PyQt5.QtWidgets import QWidget, QApplication, QMainWindow, QLabel, QVBoxLayout, QHBoxLayout, QPushButton
from PyQt5.QtGui import QPixmap, QImage
from PyQt5.QtCore import QTimer

class Ball_Game(QMainWindow):
    def __init__(self):
        super().__init__()
        self.score = 0
        self.img_width = 40
        self.img_height = 40
        
        self.img = np.zeros((self.img_width,self.img_height), dtype=np.uint8)
        self.p_x = []
        self.p_y = []
        for j in range(8):
            self.p_x.append(self.img_height-1)
            self.p_y.append(10+j)
        self.img[self.p_x,self.p_y] = 255

        self.r_x = 20
        self.r_y = 20
        self.img[self.r_x,self.r_y] = 180

        qtImage = QImage(self.img.data, self.img_width, self.img_height, QImage.Format_Grayscale8)
        pixmap = QPixmap.fromImage(qtImage)
        pixmap = pixmap.scaled(400, 400)
        
        self.image_label = QLabel()
        self.image_label.setPixmap(pixmap)

        self.score_label = QLabel("Score = "+str(0))
        self.left_button = QPushButton("Left")
        self.right_button = QPushButton("Right")

        layout_left_right = QHBoxLayout()
        layout_left_right.addWidget(self.left_button)
        layout_left_right.addWidget(QLabel(""))
        layout_left_right.addWidget(self.right_button)

        layout_left_right = QHBoxLayout()
        layout_left_right.addWidget(self.left_button)
        layout_left_right.addWidget(QLabel(""))
        layout_left_right.addWidget(self.right_button)

        control_layout = QVBoxLayout()
        control_layout.addWidget(self.score_label)
        control_layout.addWidget(QLabel(""))
        control_layout.addLayout(layout_left_right)
        control_layout.addWidget(QLabel(""))
        control_layout.addWidget(QLabel(""))
        
        main_layout = QHBoxLayout()
        main_layout.addWidget(self.image_label)
        main_layout.addLayout(control_layout)

        central_widget = QWidget(self)
        central_widget.setLayout(main_layout)
        self.setCentralWidget(central_widget)

        # Timer to update video frames
        self.timer = QTimer(self)
        self.timer.timeout.connect(self.update_game)
        self.timer_interval = 100  # Default timer interval (100 ms)
        self.timer.start(self.timer_interval)  # Update video frames every 100 ms by default

        self.direction = "NE" # "NE" , "NW" , "SE" , "SW"

        self.left_button.clicked.connect(self.left_shift)
        self.right_button.clicked.connect(self.right_shift)
    
    def update_game(self):
        self.img[self.r_x,self.r_y] = 0
        
        ### add new point
        if  self.direction=="NE":
            self.r_x = self.r_x - 1
            self.r_y = self.r_y + 1
        elif self.direction=="NW":
            self.r_x = self.r_x - 1
            self.r_y = self.r_y - 1
        elif self.direction=="SE":
            self.r_x = self.r_x + 1
            self.r_y = self.r_y + 1
        elif self.direction=="SW":
            self.r_x = self.r_x + 1
            self.r_y = self.r_y - 1

        if self.r_y==0:
            if self.r_x==0:
                if self.direction=="NW":
                    self.direction = "SE"
            elif self.direction=="NW":
                self.direction = "NE"
            elif self.direction=="SW":
                self.direction = "SE"
        elif self.r_y==self.img_width-1:
            if self.r_x==0:
                if self.direction=="NE":
                    self.direction = "SW"
            elif self.direction=="NE":
                self.direction = "NW"
            elif self.direction=="SE":
                self.direction = "SW"
        elif self.r_x==0:
            if self.direction=="NW":
                self.direction = "SW"
            elif self.direction=="NE":
                self.direction = "SE"
        elif self.r_x==self.img_height-1:
            if self.r_y>=self.p_y[0] and self.r_y<=self.p_y[-1]:
                if self.direction=="SW":
                    self.direction = "NW"
                elif self.direction=="SE":
                    self.direction = "NE"
                self.score = self.score + 1
            else:
                self.timer.stop()
                print("Game over!!!")
        
        self.img[self.r_x,self.r_y] = 180

        qtImage = QImage(self.img.data, self.img_width, self.img_height, QImage.Format_Grayscale8)
        pixmap = QPixmap.fromImage(qtImage)
        pixmap = pixmap.scaled(400, 400)
        self.image_label.setPixmap(pixmap)
        
    def right_shift(self):
        if self.p_y[-1]<self.img_width-1:
            self.img[self.p_x[0],self.p_y[0]] = 0
            self.p_y.pop(0)
            self.p_y.append(self.p_y[-1]+1)
            self.img[self.p_x[-1],self.p_y[-1]] = 255

    def left_shift(self):
        if self.p_y[0]>0:
            self.img[self.p_x[-1],self.p_y[-1]] = 0
            self.p_y.pop(-1)
            self.p_y.insert(0, self.p_y[0]-1)
            self.img[self.p_x[0],self.p_y[0]] = 255


    _
    Default Usage:
    1-Python
    2-Libraries python(Numpy,PyQt)
