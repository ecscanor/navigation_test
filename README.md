ROS Navigation Stack
====================

A 2D navigation stack that takes in information from odometry, sensor
streams, and a goal pose and outputs safe velocity commands that are sent
to a mobile base.

 * AMD64 Debian Job Status: [![Build Status](http://build.ros.org/buildStatus/icon?job=Mbin_uB64__navigation__ubuntu_bionic_amd64__binary)](http://build.ros.org/job/Mbin_uB64__navigation__ubuntu_bionic_amd64__binary/)

Related stacks:

 * http://github.com/ros-planning/navigation_msgs (new in Jade+)
 * http://github.com/ros-planning/navigation_tutorials
 * http://github.com/ros-planning/navigation_experimental

For discussion, please check out the
https://groups.google.com/group/ros-sig-navigation mailing list.
ROS 1 Navigation Stack - Code to Hardware Workflow

ROS 1 Navigation Stack
│
├── STARTUP
│   ├── map_server ──────── 載入已知地圖 (.yaml + .pgm)
│   ├── 感測器驅動 ──────── 發布 /scan, /odom
│   └── amcl ────────────── 定位，發布 TF map→odom→base_link
│
├── COSTMAP
│   ├── Global Costmap ──── 全地圖範圍，給路徑規劃用
│   └── Local Costmap ───── 機器人周圍，給即時避障用
│
├── NAVIGATION LOOP
│   │
│   ├── 1. 使用者給目標點 (RViz / topic)
│   │
│   ├── 2. Global Planner
│   │       └── A* 搜尋 Global Costmap
│   │               └── 輸出 /plan（完整路線）
│   │
│   ├── 3. Local Planner (@ 20Hz)
│   │       └── DWA 取樣速度候選
│   │               └── 輸出 /cmd_vel（線速度 + 角速度）
│   │
│   ├── 4. 機器人移動
│   │
│   └── 5. 感測器閉迴路
│           ├── Encoder → /odom → amcl 更新定位
│           └── LiDAR  → /scan → costmap 更新障礙物
│                                   └── 回到步驟 3
│
├── EXCEPTION (卡住時)
│   ├── rotate_recovery ─── 原地旋轉重新掃描
│   ├── clear_costmap ────── 清除障礙重新規劃
│   ├── move_slow_and_clear  慢速前進
│   └── 全失敗 ───────────── ABORTED
│
└── GOAL REACHED
        └── 進入容忍範圍 → /cmd_vel = 0 → SUCCEEDED