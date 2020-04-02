# Симулятор px4 SITL + clover + gazebo

1. Настройка и установка тулчейна ROS Melodic + необходимые библиотеки для компиляции прошивки:
    [https://dev.px4.io/v1.8.2/en/setup/dev\_env\_linux.html\#gazebo-with-ros](https://dev.px4.io/v1.8.2/en/setup/dev_env_linux.html#gazebo-with-ros)

2. Установка библиотек для gstreamer v1.0 (если будут ошибки GSTREAMER_LIBRARIES при сборке симулятора):
    [https://github.com/PX4/Firmware/issues/13117\#issuecomment-539400429](https://github.com/PX4/Firmware/issues/13117#issuecomment-539400429)

3. Сборка симулятора (для версии v1.8.2-clever):

```cmd
git clone https://github.com/CopterExpress/Firmware.git
cd <coex px4 firmware fork>
git checkout v1.8.2-clever
git submodule update --init --recursive
make posix_sitl_default gazebo
```

4. Для того, чтобы файлы симулятора стали видны для ROS, нужно добавить в конец ~/.bashrc:

```cmd
source <coex px4 firmware fork>/Tools/setup\_gazebo.bash <coex px4 firmware fork> <coex px4 firmware fork>/build/posix\_sitl\_default
export ROS\_PACKAGE\_PATH=$ROS\_PACKAGE\_PATH:<coex px4 firmware fork>
export ROS\_PACKAGE\_PATH=$ROS\_PACKAGE\_PATH:<coex px4 firmware fork>/Tools/sitl\_gazebo
```

5. Настройки для коптера при запусе SITL находятся [здесь](https://github.com/CopterExpress/Firmware/tree/v1.8.2-clever/posix-configs/SITL/init)

6. launch файлы для запуска симулятора находятся [здесь](https://github.com/CopterExpress/Firmware/tree/v1.8.2-clever/launch)

7. Запуск связки px4 SITL + gazebo:

```cmd
roslaunch px4 posix_sitl.launch
```

8. Сборка клевера для ROS Melodic:

```cmd
cd catkin_ws/src
git clone https://github.com/CopterExpress/clever.git
git clone https://github.com/CopterExpress/ros_led.git
cd ..
rosdep install -y --from-paths src --ignore-src -r
catkin_make
. devel/setup.bash
```

9. Установка данного репозитория:

```cmd
mkdir -p ~/sim_ws/src
cd ~/sim_ws/src
git clone https://github.com/goldarte/clover-gazebo-single.git
cd ..
catkin_make
. devel/setup.bash
```

Для автоматического обновления окружения нового терминала нужно добавить в ~/.bashrc:

```bash
source ~/sim_ws/devel/setup.bash
```

10. Запуск связки px4 SITL (LPE + gps) + clover (simple offboard + mavros) + gazebo:

```cmd
roslaunch clover-gazebo-single sitl_gps_lpe.launch
```
