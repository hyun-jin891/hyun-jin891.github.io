---
layout: post
title: KDEL Visualization
description: >
  Model for relationship between ER Chaperone and KDEL Receptor
tags: [project]
use_math: true
categories:
  - Project
---

### KDEL Visualization
ER Chaperone - KDEL Receptor 사이의 관계를 시각화한다.

### Purpose
* 세포생물학에서 공부했던 것 중에 인상 깊었던 내용 중 하나가 **ER Chaperone**과 **KDEL Receptor**의 interaction이었다.
* 이 interaction으로 파생되는 결과를 시각화를 통해 파악해보고 싶었다.
* 또한 이 시각화 모델을 통해 KDEL amino acid sequence의 존재 의미를 파악할 수 있다.

### Implementation

* 구현한 cellWorld(클래스) 안에는 장소를 3개로 구분하였다.<br>
  1) ER (왼쪽)

  2) 세포질 (ER 경계막과 Golgi 경계막 사이 공간)

  3) Golgi (오른쪽)

* 구현한 cellWorld(클래스)에서 움직일 객체의 종류는 총 5가지이다.<br>

  1) **ER Chaperones** : 아직 가공이 되지 않은 unfolded protein이 folding될 수 있도록 하는 객체

  2) **Unfolded Proteins** : 아직 가공이 되지 않은 protein 객체

  3) **Other Proteins** : 그 외의 protein 객체

  4) **KDEL Receptors** : ER Chaperones을 인지하면 Golgi에서 ER로 이동하는 vesicle이 만들어지도록 하는 객체

  5) **Other Receptors** : Other proteins을 인지하면 ER에서 Golgi로 이동하는 vesicle이 만들어지도록 하는 객체

* matrix (self.__map) 안의 값을 변화시켜 각 객체의 위치를 계속 업데이트 시킨다.
  * self.__map[i][j] == 0 : 아무 것도 없음 (**white**)
  * self.__map[i][j] == 1 : ER Chaperone (**dark green**)
  * self.__map[i][j] == 2 : Unfolded Protein (**royalblue**)
  * self.__map[i][j] == 3 : Other Proteins (**black**)
  * self.__map[i][j] == 4 : KDEL Receptor (**aquamarine**)
  * self.__map[i][j] == 5 : Other Receptors (**darkgray**)
  * self.__map[i][j] == 6 : ER에서 Golgi로 이동하고 있는 객체 (**red**)
  * self.__map[i][j] == 7 : Golgi에서 ER로 이동하고 있는 객체 (**blue**)

* Vesicle의 이동 (6 or 7)
  * Receptor가 각자가 인지할 수 있는 객체를 인지하면 vesicle이 형성될 수 있는데 이때 vesicle 안에 들어가 이동할 수 있는 객체는 그 Receptor와 인식된 객체뿐만 아니라 **근처에 존재하는 아무 객체**나 그 vesicle 안에 들어갈 수 있다.
  * 매 턴마다 x축으로 1칸씩 이동한다. (6이면 +1, 7이면 -1)
  * vesicle이 막에 도착하면 이동한 receptor는 막에 위치하게 되고 그 외 객체는 막 안쪽 근처에 위치하게 된다.

* ER Chaperones (1)
  * 초기화 생성은 모두 ER에서 이뤄지도록 한다. (초기화 했을 때의 위치)
  * 초기화 이후부터는 생성되지 않게 설정 (실제로는 생성됨)
  * vesicle 안에 담겨 이동하는 상황을 제외하고는 매 turn마다 상하좌우로 랜덤하게 움직인다. (또는 안 움직임)
  * 주위에 unfolded protein이 있으면 제거한다.

* Unfolded Proteins (2)
  * 초기화 생성은 ER과 Golgi 모두에서 이뤄지도록 한다. (Golgi에 있는 경우는 vesicle에 우연히 담겨져서 온 경우이기 때문에 비율은 ER에서 생성되는게 많도록 한다.)
  * ER Chaperone에 의해 제거되어 숫자가 0이 되는 것을 방지하기 위해 ER Chaperone과는 다르게 초기화 이후에도 작은 확률로 ER에서 생성될 수 있도록 한다.
  * vesicle 안에 담겨 이동하는 상황을 제외하고는 매 turn마다 상하좌우로 랜덤하게 움직인다. (또는 안 움직임)

* Other Proteins (3)
  * 초기화 생성은 ER과 Golgi 모두에서 이뤄지도록 한다.
  * vesicle 안에 담겨 이동하는 상황을 제외하고는 매 turn마다 상하좌우로 랜덤하게 움직인다. (또는 안 움직임)

* KDEL Receptors (4)
  * 초기화 생성은 Golgi의 경계막에서 이뤄지도록 한다.
  * 초기화 이후에도 작은 확률로 ER에서 생성될 수 있도록 한다.
  * vesicle 안에 담겨 이동하는 상황을 제외하고는 매 turn마다 상하 중 랜덤하게 ER 또는 Golgi의 경계막에서 움직인다. (또는 안 움직임)
  * Golgi 경계막에 위치하고 있을 때 ER Chaperone을 인지하면 자신과 그 Chaperone을 포함하여 그 근처에 존재하는 모든 객체의 행렬에서의 값을 7(blue)로 바꾼다.

* Other Receptors (5)
  * 초기화 생성은 ER의 경계막에서 이뤄지도록 한다.
  * 초기화 이후에도 작은 확률로 ER에서 생성될 수 있도록 한다.
  * vesicle 안에 담겨 이동하는 상황을 제외하고는 매 turn마다 상하 중 랜덤하게 ER 또는 Golgi의 경계막에서 움직인다. (또는 안 움직임)
  * ER 경계막에 위치하고 있을 때 other protein을 인지하면 자신과 그 other protein을 포함하여 그 근처에 존재하는 모든 객체의 행렬에서의 값을 6(red)로 바꾼다.


### Code

~~~python
from random import *
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import matplotlib.colors as colors


ER_MEMBRANE = 19
GOLGI_MEMBRANE = 80
CHAPERONE_SPEED = 0.7
UNFOLED_PROTEIN_SPEED = 0.3
OTHER_PROTEIN_SPEED = 0.7
MEMBRANE_SPEED = 0.5
UNFOLDED_PROTEIN_GENERATION = 0.3
KDEL_RECEPTOR_GENERATION = 0.002
OTHER_RECEPTOR_GENERATION = 0.1
KDEL_RECEPTOR_AFFINITY = 0.8


class Resident_Soluble_Protein:
    def __init__(self, x, y):
        self._x = x
        self._y = y

    def setterXY(self, x, y):
        self._x = x
        self._y = y

    def getterX(self):
        return self._x

    def getterY(self):
        return self._y

    def move(self):
        while True:
            change_X = sample([-1, 1], 1)[0]
            change_Y = sample([-1, 1], 1)[0]
            if 2 <= self._y + change_Y <= 97:
                if (1 <= self._x + change_X < ER_MEMBRANE) or (GOLGI_MEMBRANE < self._x + change_X <= 98):
                    self._x += change_X
                    self._y += change_Y
                    return

class ER_Chaperone(Resident_Soluble_Protein):
    pass


class Unfolded_Protein(Resident_Soluble_Protein):
    pass


class Other_Protein(Resident_Soluble_Protein):
    pass



class Receptor:
    def __init__(self, x, y):
        self._x = x
        self._y = y

    def setterXY(self, x, y):
        self._x = x
        self._y = y

    def getterX(self):
        return self._x

    def getterY(self):
        return self._y

    def move(self):
        while True:
            change = sample([-1, 1], 1)[0]
            if 2 <= self._y + change <= 97:
                self._y += change
                return

class KDEL_Receptor(Receptor):
    pass

class Other_Receptor(Receptor):
    pass            


class Cell_World:
    def __init__(self, n1, n2, n3, n4, n5):
        self.__map = [[0 for i in range(100)] for j in range(100)]
        self.er_chaperone = []
        self.unfolded_protein = []
        self.other_protein = []
        self.kdel_receptor = []
        self.other_receptor = []

        for i in range(n1):
            while True:
                random_x = randint(1, ER_MEMBRANE - 1)
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 1
                    self.er_chaperone.append(ER_Chaperone(random_x, random_y))
                    break

        for i in range(n2):
            weight = [0.8, 0.2]
            while True:
                random_x = choices([randint(1, ER_MEMBRANE - 1), randint(GOLGI_MEMBRANE + 1, 98)], weight)[0]
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 2
                    self.unfolded_protein.append(Unfolded_Protein(random_x, random_y))
                    break

        for i in range(n3):
            while True:
                random_x = sample([randint(1, ER_MEMBRANE - 1), randint(GOLGI_MEMBRANE + 1, 98)], 1)[0]
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 3
                    self.other_protein.append(Other_Protein(random_x, random_y))
                    break

        for i in range(n4):
            while True:
                random_x = GOLGI_MEMBRANE
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 4
                    self.kdel_receptor.append(KDEL_Receptor(random_x, random_y))
                    break

        for i in range(n5):
            while True:
                random_x = ER_MEMBRANE
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 5
                    self.other_receptor.append(Other_Receptor(random_x, random_y))
                    break

    def getterMap(self):
        return self.__map

    def numUnfolded_Protein(self):
        num1 = 0
        num2 = 0

        for i in range(len(self.unfolded_protein)):
            if self.unfolded_protein[i] == None:
                continue
            else:
                if 1 <= self.unfolded_protein[i].getterX() < ER_MEMBRANE:
                    num1 += 1
                elif GOLGI_MEMBRANE < self.unfolded_protein[i].getterX() <= 98:
                    num2 += 1

        return num1, num2


    def runWorld(self):
        self.unfolded_protein = list(filter(None, self.unfolded_protein))
        p = uniform(0.0, 1.0)

        if p <= UNFOLDED_PROTEIN_GENERATION:
            while True:
                random_x = randint(0, ER_MEMBRANE - 1)
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 2
                    self.unfolded_protein.append(Unfolded_Protein(random_x, random_y))
                    break

        if p <= KDEL_RECEPTOR_GENERATION:
            while True:
                random_x = ER_MEMBRANE
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 4
                    self.kdel_receptor.append(KDEL_Receptor(random_x, random_y))
                    break


        if p <= OTHER_RECEPTOR_GENERATION:
            while True:
                random_x = ER_MEMBRANE
                random_y = randint(2, 97)

                if self.__map[random_y][random_x] == 0:
                    self.__map[random_y][random_x] = 5
                    self.other_receptor.append(Other_Receptor(random_x, random_y))
                    break


        for i in range(len(self.er_chaperone)):
            current_Chaperone = self.er_chaperone[i]
            current_x = current_Chaperone.getterX()
            current_y = current_Chaperone.getterY()

            if self.__map[current_y][current_x] == 6:
                flag = False

                if self.__map[current_y][current_x + 1] != 0:
                    if self.__map[current_y][current_x + 1] == 7:
                        current_Chaperone.setterXY(current_x + 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_Chaperone.setterXY(current_x + 1, current_y)


                if current_Chaperone.getterX() == GOLGI_MEMBRANE:
                    while True:
                        current_Chaperone.move()
                        if self.__map[current_Chaperone.getterY()][current_Chaperone.getterX() + 4] == 0:
                           self.__map[current_y][current_x] = 0
                           self.__map[current_Chaperone.getterY()][current_Chaperone.getterX() + 4] = 1
                           break
                        else:
                            current_Chaperone.setterXY(GOLGI_MEMBRANE, current_y)
                    continue

                self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] = 6
                self.__map[current_y][current_x] = 0
                continue

            elif self.__map[current_y][current_x] == 7:
                flag = False
                if self.__map[current_y][current_x - 1] != 0:
                    if self.__map[current_y][current_x - 1] == 6:
                        current_Chaperone.setterXY(current_x - 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_Chaperone.setterXY(current_x - 1, current_y)


                if current_Chaperone.getterX() == ER_MEMBRANE:
                    while True:
                        current_Chaperone.move()
                        if self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] == 0:
                            self.__map[current_y][current_x] = 0
                            self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] = 1
                            break
                        else:
                            current_Chaperone.setterXY(ER_MEMBRANE, current_y)
                    continue

                self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] = 7
                self.__map[current_y][current_x] = 0
                continue

            factor = uniform(0.0, 1.0)

            if factor <= CHAPERONE_SPEED:
                current_Chaperone.move()
                if self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] != 0:
                    current_Chaperone.setterXY(current_x, current_y)
                else:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] = 1

            direction = [(-1, 0), (0, -1), (1, 0), (0, 1)]
            for j in range(4):
                search_locationX = current_Chaperone.getterX() + direction[j][0]
                search_locationY = current_Chaperone.getterY() + direction[j][1]
                if search_locationX == ER_MEMBRANE or search_locationX == GOLGI_MEMBRANE:
                    continue
                elif search_locationY < 0 or search_locationY > 99:
                    continue

                if self.__map[search_locationY][search_locationX] == 2:
                    self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] = 0
                    current_Chaperone.setterXY(search_locationX, search_locationY)
                    self.__map[current_Chaperone.getterY()][current_Chaperone.getterX()] = 1
                    break

        for i in range(len(self.unfolded_protein)):
            current_unfolded_protein = self.unfolded_protein[i]
            current_x = current_unfolded_protein.getterX()
            current_y = current_unfolded_protein.getterY()

            if self.__map[current_y][current_x] == 1:
                self.unfolded_protein[i] = None
                del current_unfolded_protein
                continue

            if self.__map[current_y][current_x] == 6:
                flag = False
                if self.__map[current_y][current_x + 1] != 0:
                    if self.__map[current_y][current_x + 1] == 7:
                        current_unfolded_protein.setterXY(current_x + 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_unfolded_protein.setterXY(current_x + 1, current_y)


                if current_unfolded_protein.getterX() == GOLGI_MEMBRANE:
                    while True:
                        current_unfolded_protein.move()
                        if self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] == 0:
                           self.__map[current_y][current_x] = 0
                           self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] = 2
                           break
                        else:
                            current_unfolded_protein.setterXY(GOLGI_MEMBRANE, current_y)
                    continue

                self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] = 6
                self.__map[current_y][current_x] = 0
                continue

            elif self.__map[current_y][current_x] == 7:
                flag = False
                if self.__map[current_y][current_x - 1] != 0:
                    if self.__map[current_y][current_x - 1] == 6:
                        current_unfolded_protein.setterXY(current_x - 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_unfolded_protein.setterXY(current_x - 1, current_y)


                if current_unfolded_protein.getterX() == ER_MEMBRANE:
                    while True:
                        current_unfolded_protein.move()
                        if self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] == 0:
                            self.__map[current_y][current_x] = 0
                            self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] = 2
                            break
                        else:
                            current_unfolded_protein.setterXY(ER_MEMBRANE, current_y)
                    continue

                self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] = 7
                self.__map[current_y][current_x] = 0
                continue

            factor = uniform(0.0, 1.0)

            if factor <= UNFOLED_PROTEIN_SPEED:
                current_unfolded_protein.move()
                if self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] != 0:
                    current_unfolded_protein.setterXY(current_x, current_y)
                else:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_unfolded_protein.getterY()][current_unfolded_protein.getterX()] = 2


        for i in range(len(self.other_protein)):
            current_other_protein = self.other_protein[i]
            current_x = current_other_protein.getterX()
            current_y = current_other_protein.getterY()

            if self.__map[current_y][current_x] == 6:
                flag = False
                if self.__map[current_y][current_x + 1] != 0:
                    if self.__map[current_y][current_x + 1] == 7:
                        current_other_protein.setterXY(current_x + 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_other_protein.setterXY(current_x + 1, current_y)


                if current_other_protein.getterX() == GOLGI_MEMBRANE:
                    while True:
                        current_other_protein.move()
                        if self.__map[current_other_protein.getterY()][current_other_protein.getterX()] == 0:
                           self.__map[current_y][current_x] = 0
                           self.__map[current_other_protein.getterY()][current_other_protein.getterX()] = 3
                           break
                        else:
                            current_other_protein.setterXY(GOLGI_MEMBRANE, current_y)
                    continue

                self.__map[current_other_protein.getterY()][current_other_protein.getterX()] = 6
                self.__map[current_y][current_x] = 0
                continue

            elif self.__map[current_y][current_x] == 7:
                flag = False
                if self.__map[current_y][current_x - 1] != 0:
                    if self.__map[current_y][current_x - 1] == 6:
                        current_other_protein.setterXY(current_x - 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_other_protein.setterXY(current_x - 1, current_y)

                if current_other_protein.getterX() == ER_MEMBRANE:
                    while True:
                        current_other_protein.move()
                        if self.__map[current_other_protein.getterY()][current_other_protein.getterX() + 4] == 0:
                            self.__map[current_y][current_x] = 0
                            self.__map[current_other_protein.getterY()][current_other_protein.getterX() + 4] = 3
                            break
                        else:
                            current_other_protein.setterXY(ER_MEMBRANE, current_y)
                    continue

                self.__map[current_other_protein.getterY()][current_other_protein.getterX()] = 7
                self.__map[current_y][current_x] = 0
                continue

            factor = uniform(0.0, 1.0)

            if factor <= OTHER_PROTEIN_SPEED:
                current_other_protein.move()
                if self.__map[current_other_protein.getterY()][current_other_protein.getterX()] != 0:
                    current_other_protein.setterXY(current_x, current_y)
                else:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_other_protein.getterY()][current_other_protein.getterX()] = 3

        for i in range(len(self.kdel_receptor)):
            current_kdel_receptor = self.kdel_receptor[i]
            current_x = current_kdel_receptor.getterX()
            current_y = current_kdel_receptor.getterY()

            if self.__map[current_y][current_x] == 6:
                flag = False

                if self.__map[current_y][current_x + 1] != 0:
                    if self.__map[current_y][current_x + 1] == 7:
                        current_kdel_receptor.setterXY(current_x + 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_kdel_receptor.setterXY(current_x + 1, current_y)


                if current_kdel_receptor.getterX() == GOLGI_MEMBRANE:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_y][current_kdel_receptor.getterX()] = 4
                    continue

                self.__map[current_kdel_receptor.getterY()][current_kdel_receptor.getterX()] = 6
                self.__map[current_y][current_x] = 0
                continue

            elif self.__map[current_y][current_x] == 7:
                flag = False

                if self.__map[current_y][current_x - 1] != 0:
                    if self.__map[current_y][current_x - 1] == 6:
                        current_kdel_receptor.setterXY(current_x-3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_kdel_receptor.setterXY(current_x - 1, current_y)

                if current_kdel_receptor.getterX() == ER_MEMBRANE:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_y][current_kdel_receptor.getterX()] = 4
                    continue

                self.__map[current_kdel_receptor.getterY()][current_kdel_receptor.getterX()] = 7
                self.__map[current_y][current_x] = 0
                continue

            factor = uniform(0.0, 1.0)

            if factor <= MEMBRANE_SPEED:
                current_kdel_receptor.move()
                if self.__map[current_kdel_receptor.getterY()][current_kdel_receptor.getterX()] != 0:
                    current_kdel_receptor.setterXY(current_x, current_y)
                else:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_kdel_receptor.getterY()][current_kdel_receptor.getterX()] = 4

            if current_kdel_receptor.getterX() == GOLGI_MEMBRANE:
                p = uniform(0.0, 1.0)
                if self.__map[current_kdel_receptor.getterY()][GOLGI_MEMBRANE + 1] == 1 and p <= KDEL_RECEPTOR_AFFINITY:
                    self.__map[current_kdel_receptor.getterY()][GOLGI_MEMBRANE] = 7
                    self.__map[current_kdel_receptor.getterY()][GOLGI_MEMBRANE + 1] = 7

                    direction = [(0, -1), (0, 1), (1, -1), (1, 1)]

                    for j in range(len(direction)):
                        search_locationX = current_kdel_receptor.getterX() + direction[j][0]
                        search_locationY = current_kdel_receptor.getterY() + direction[j][1]
                        if self.__map[search_locationY][search_locationX] != 0:
                            self.__map[search_locationY][search_locationX] = 7       


        for i in range(len(self.other_receptor)):
            current_other_receptor = self.other_receptor[i]
            current_x = current_other_receptor.getterX()
            current_y = current_other_receptor.getterY()

            if self.__map[current_y][current_x] == 6:
                flag = False

                if self.__map[current_y][current_x + 1] != 0:
                    if self.__map[current_y][current_x + 1] == 7:
                        current_other_receptor.setterXY(current_x + 3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_other_receptor.setterXY(current_x + 1, current_y)


                if current_other_receptor.getterX() == GOLGI_MEMBRANE:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_y][current_other_receptor.getterX()] = 5
                    continue

                self.__map[current_other_receptor.getterY()][current_other_receptor.getterX()] = 6
                self.__map[current_y][current_x] = 0
                continue

            elif self.__map[current_y][current_x] == 7:
                flag = False

                if self.__map[current_y][current_x - 1] != 0:
                    if self.__map[current_y][current_x - 1] == 6:
                        current_other_receptor.setterXY(current_x-3, current_y + 2)
                        flag = True
                    else:
                        continue
                if flag == False:
                    current_other_receptor.setterXY(current_x - 1, current_y)


                if current_other_receptor.getterX() == ER_MEMBRANE:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_y][current_other_receptor.getterX()] = 5
                    continue

                self.__map[current_other_receptor.getterY()][current_other_receptor.getterX()] = 7
                self.__map[current_y][current_x] = 0
                continue

            factor = uniform(0.0, 1.0)

            if factor <= MEMBRANE_SPEED:
                current_other_receptor.move()
                if self.__map[current_other_receptor.getterY()][current_other_receptor.getterX()] != 0:
                    current_other_receptor.setterXY(current_x, current_y)
                else:
                    self.__map[current_y][current_x] = 0
                    self.__map[current_other_receptor.getterY()][current_other_receptor.getterX()] = 5

            if current_other_receptor.getterX() == ER_MEMBRANE:
                if self.__map[current_other_receptor.getterY()][ER_MEMBRANE - 1] == 3:
                    self.__map[current_other_receptor.getterY()][ER_MEMBRANE] = 6
                    self.__map[current_other_receptor.getterY()][ER_MEMBRANE - 1] = 6

                    direction = [(0, -1), (0, 1), (-1, -1), (-1, 1)]

                    for j in range(len(direction)):
                        search_locationX = current_other_receptor.getterX() + direction[j][0]
                        search_locationY = current_other_receptor.getterY() + direction[j][1]
                        if self.__map[search_locationY][search_locationX] != 0:
                            self.__map[search_locationY][search_locationX] = 6    

def onClick(event):
    global pause
    pause ^= True
    anim.event_source.stop()

def update(i):
    print(miniCellWorld.numUnfolded_Protein())
    miniCellWorld.runWorld()
    matrix = np.matrix(miniCellWorld.getterMap())
    ax.cla()
    im = ax.matshow(matrix, cmap = cmap, norm = norm)
    plt.vlines(ER_MEMBRANE, 0, 99, color='black', linestyle='solid', linewidth=0.5)
    plt.vlines(GOLGI_MEMBRANE, 0, 99, color='black', linestyle='solid', linewidth=0.5)
    fig.tight_layout()

    return [im]


n1, n2, n3, n4, n5 = map(int, input().split())               


pause = False

miniCellWorld = Cell_World(n1, n2, n3, n4, n5)

matrix = np.matrix(miniCellWorld.getterMap())

fig, ax = plt.subplots()

y = np.arange(0, 99)

plt.vlines(ER_MEMBRANE, 0, 99, color='black', linestyle='solid', linewidth=0.5)
plt.vlines(GOLGI_MEMBRANE, 0, 99, color='black', linestyle='solid', linewidth=0.5)

fig.set_figheight(5)
fig.set_figwidth(5)

fig.canvas.mpl_connect('button_press_event', onClick)
fig.tight_layout()

cmap = colors.ListedColormap(['white', 'darkgreen', 'royalblue', 'black', 'aquamarine', 'darkgray', 'red', 'blue'])
norm = colors.BoundaryNorm(np.arange(len(matrix) + 1) - 0.5, len(matrix))
im = ax.matshow(matrix, cmap = cmap, norm = norm)

anim = animation.FuncAnimation(fig, update)

plt.show()
~~~
<br>

### Result
ER Chaperone : 100개, Unfolded Proteins : 200개, Other Proteins : 70개, KDEL Receptors : 20개, Other Receptors : 10개로 입력하고 실행하였다. <br>

* Other Receptors가 other proteins을 인지하여 발생한 vesicles이 Golgi로 이동하고 있는 모습이다.<br>

![그림1](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/144.PNG?raw=true){: width="1500" height="400"}<br>


![그림2](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/146.PNG?raw=true){: width="1500" height="400"}<br>

* (a, b) : a는 ER에서의 unfolded protein 개수, b는 Golgi에서의 unfolded protein 개수이다.
* 처음 초기화했을 때 a가 b보다 훨씬 많았음에도 ER에는 Chaperone이 많기 때문에 a의 값은 금방 줄어드는 것을 확인할 수 있다.
* b의 값은 혹여나 vesicle에 우연히 담겨져 Golgi로 이동했을 가능성을 고려한 것이다.
* b의 값이 감소하고 있는 것을 통해 Other Receptors에 의해 만들어진 vesicle 안에 우연히 담겨져 Golgi로 이동한 ER Chaperone이 Golgi에 있는 unfolded protein을 제거하고 있는 것을 확인할 수 있다.<br>

* KDEL Receptors가 ER Chaperones을 인지하여 발생한 vesicles이 ER로 이동하고 있는 모습이다.<br>

![그림3](https://github.com/hyun-jin891/hyun-jin891.github.io/blob/master/assets/img/145.PNG?raw=true){: width="1500" height="400"}<br>


### Conclusion
* Unfolded proteins은 ER Chaperones에 의해 folding이 되어야 하는 객체이다.
* 보통의 경우라면 ER에서 folding이 이루어진다.
* 하지만 ER에서 만들어진 vesicle에 우연히 unfolded proteins이 담겨 Golgi로 이동하게 될 수 있다.
* 다행히 ER Chaperones의 경우에도 우연히 vesicle에 담겨져 Golgi로 이동할 수 있다.
* 따라서 Golgi로 탈출한 unfolded proteins을 ER Chaperones이 잡을 수 있게 되는 것이다.
* Golgi로 이동한 ER Chaperones는 다시 ER로 복귀해야 한다.
* **복귀를 위해 있는 것이 KDEL Receptors인 것이다**.


### Limitation
* 각 객체의 생리적 활성, 생성, 움직임 등의 특징들을 생물학 이론에 근거해 최대한 반영하려고 하였지만 일부는 프로그래머가 **임의로 확률**을 정해서 하였기 때문에 정확성이 떨어진다.

* 객체가 움직일 때 **랜덤하게** 상하(좌우)로 움직이기 때문에 드라마틱하게 이동하지 못하고 주위에서 맴돌 때가 많다. (그래서 ER Chaperone이 Golgi에 도착하자마자 바로 ER로 가는 vesicle에 담기는 것을 막기 위해 도착하면 막에서 조금 떨어진 곳에 위치 시키기도 하였다.)

* 도착한 막에 객체들이 많아 빈틈이 안 생기면 vesicle에 담겨 있는 객체가 들어가지 못하고 틈이 생길 때까지 기다리는 현상이 발생함.
