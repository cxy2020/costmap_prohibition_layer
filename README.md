costmap_prohibition_layer
======================
# 1. 概述
costmap_prohibition_layer是基于ros-package "costmap_prohibition_layer"修改。继承自costmap_2d::CostmapLayer，提供了对禁区的添加和删除等操作的功能。<br>
ros-package "costmap_prohibition_layer"相关资料参考: http://wiki.ros.org/costmap_prohibition_layer<br>

# 2. 主要改动
## 2.1. 区域更新范围
禁区设置的范围在地图上分布较广，如果在updateBounds时直接将禁区范围更新到costmap中，会导致其他layer进行不必要的区域更新，导致地图更新很慢。<br>
为了节省计算量，不将禁区的范围更新到costmap中，而只在禁区更新updateCosts时使用。<br>
## 2.2. 增加添加和删除禁区的Service
目前提供了3个Service用来对禁区进行操作：<br>

* AddProhibitionZone<br>
参数：prohibition zone的边缘上顶点<br>
输出：添加区域的生成id <br>

* ClearProhibitionZone<br>
参数：无<br>
输出：无<br>

* RemoveProhibitionZone<br>
参数：待删除区域的id<br>
输出：无<br>

## 2.3. 禁区id的分配和维护
禁区id的分配主要分以下几种情况：<br>

* 当前分配的最大id<UINT32_MAX（32位无符号整型最大值): <br>
添加新的禁区时，分配id为当前最大id+1 <br>
* 当前分配最大id=UINT32_MAX：<br>
从禁区列表头部开始搜索，寻找最小可用的id号作为新的禁区id<br>
