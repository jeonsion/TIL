# [SLAM] Odometry

## What is Odometry?

`주행기록계라는` 의미로 `엔코더를` 통한 `회전수`, `IMU로 기울기` 등을 측정함으로써 `사물의 위치를 측정하는 방법`이다.

Odometry는 ROS에서 odom frame으로 구현되며 위에서 설명한 엔코더 혹은 IMU를 사용하여 위치를 추정할 수 있다. *부득이하게 Visual SLAM을 사용하여 위의 센서를 사용하지 못할 경우 카메라를 통해 관측한 값을 토대로 Odom을 추정하는 방법을 사용할 수 있다.

![스크린샷 2023-02-15 오후 11.39.40.png](%5BSLAM%5D%20Odometry%204ee5ce6142504acd9d55f2fe3ea82f19/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-02-15_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_11.39.40.png)

Odom frame을 설계함에 있어 일반적으로 위와 같은 구조로 Frame을 설계한다.

map은 여러개의 odom을 가질 수 있으나 odom은 하나의 map을 가질 수 있다(종속관계). 

> 즉 1개의 map 위에 다수의 로봇이 존재할 경우 odom은 로봇의 개수만 큼 존재
> 

### earth

`earth frame`은 여러대의 로봇들이 서로 다른 `map`위에서 존재할 수 있도록 해준다.

로봇들이 한 빌딩의 각 층에 위치하여 서로다른 map을 가지고 있을 경우, earth  frame은 모든 map frame을 통해 각 층의 정보를 가질 수 있게 해준다.

### map

map frame은 `world fixed frame`으로서 earth frame에 고정된 프레임이라 할 수 있다.
odom frame이 움직일 때 drift가 발생하여 로봇의 위치가 미세하게 바뀌는 경우가 발생한다. map frame은 연속적이지 않기 때문에 시뮬레이션을 돌릴 때 마치 로봇이 갑자기 순간이동 한 듯 움직이는 경우를 볼 수 있습니다. 이러한 단점이 있지만 map frame은 장기간 `global reference`에는 매우 유용한 frame입니다.

> *drift = 우리가 생각하는 그 드리프트
> 

### odom

odom frame은 world fixed frame으로서 또한 earth frame에 고정된 프레임이라 할 수 있다. 로봇의 위치가 drift로 인해 odom frame이 장기간 global reference에서는 사용에 있어 의미가 없을 수 있으나 odom frame은 로봇의 위치를 연속적으로 나타낼 수 있어 로봇이 갑자기 순간이동을 하는 듯한 모습을 보이지 않고 `부드럽게 움직일 수 있도록 해준다`.

즉 odom frame은 단기간 local reference에서 로봇의 위치를 정확하게 파악하는데는 매우 유용한 frame이라 할 수 있다.

### base_link

base_link frame은 `로봇 자체의 위치를 나타내는 frame`이라 할 수 있다. base_link frame의 위치를 odom 혹은 map frame을 사용하여 위치를 추정하는 방법으로 로봇의 위치를 파악할 수 있다.
