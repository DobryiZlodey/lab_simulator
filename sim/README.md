📌 Описание проекта
Этот проект содержит:

Мир лаборатории для Gazebo Harmonic (lab_world.sdf)

Модель робота Pioneer3AT, исправленную под SDFormat 1.7

Поддержку дифференциального привода через gz::sim::systems::DiffDrive

Интеграцию с ROS 2 Jazzy через топик /model/robot/cmd_vel

Полностью рабочий пайплайн: запуск мира → загрузка робота → управление движением

📂 Структура проекта
```bash
ros2_ws/
└── src/
    └── lab_world/
        ├── worlds/
        │   └── lab_world.sdf
        ├── models/
        │   └── pioneer3at/
        │       ├── model.sdf
        │       ├── model.config
        │       └── meshes/
        │           ├── chassis.dae
        │           └── wheel.dae
        └── README.md
```
Модель также должна быть установлена в:

```bash
~/.gz/models/pioneer3at/
```

🛠 Требования
ROS 2 Jazzy

Gazebo Harmonic (gz-sim 8)

colcon для сборки

Ubuntu 22.04 / 24.04

🚀 Установка
1. Установить Gazebo Harmonic

```bash
sudo apt install gz-harmonic
```
2. Установить ROS 2 Jazzy
(если ещё не установлен)

```bash
sudo apt install ros-jazzy-desktop
```
3. Склонировать проект

```bash
cd ~/ros2_ws/src
git clone <your_repo_url> lab_world
```
4. Установить модель в Gazebo

```bash
mkdir -p ~/.gz/models/pioneer3at
cp -r ros2_ws/src/lab_world/models/pioneer3at/* ~/.gz/models/pioneer3at/
```
5. Собрать рабочее пространство

```bash
cd ~/ros2_ws
colcon build
source install/setup.bash
```
▶️ Как запустить симуляцию
1. Запуск Gazebo с миром

```bash
gz sim ~/ros2_ws/src/lab_world/worlds/lab_world.sdf
```
После запуска в мире появится:

лаборатория

робот pioneer3at с именем robot

🎮 Управление роботом
Робот использует стандартный ROS 2 топик:

```bash
/model/robot/cmd_vel
```
Пример: ехать вперёд

```bash
ros2 topic pub -r 10 /model/robot/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.5}, angular: {z: 0.0}}"
```
Повернуть

```bash
ros2 topic pub -r 10 /model/robot/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.0}, angular: {z: 1.0}}"
```
Остановиться

```bash
ros2 topic pub /model/robot/cmd_vel geometry_msgs/msg/Twist "{}"
```
🧪 Проверка, что всё работает
1. Проверить, что робот загружен
   
```bash
gz model --list
```
Ожидаемый вывод:

```bash
lab
robot
```
2. Проверить, что джойнты созданы
   
```bash
gz service -l | grep joint
```
Ожидаемый вывод:

```bash
/model/robot/joint/right_front_joint
/model/robot/joint/left_front_joint
/model/robot/joint/right_rear_joint
/model/robot/joint/left_rear_joint
```
3. Проверить, что DiffDrive активен
В логах Gazebo должно быть:

```bash
DiffDrive subscribing to twist messages on [/model/robot/cmd_vel]
```
🛠 Отладка
Если робот не двигается:
Проверить, что топик существует:

```bash
ros2 topic list | grep cmd_vel
```
Проверить, что Gazebo видит джойнты:

```bash
gz service -l | grep joint
```
Проверить, что модель корректно установлена:

```bash
ls ~/.gz/models/pioneer3at
```
Проверить, что inertia корректна (все 6 элементов):

```bash
ixx, ixy, ixz, iyy, iyz, izz
```