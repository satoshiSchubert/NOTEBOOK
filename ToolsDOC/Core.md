# DOC FOR RAYTOOLS

## core

核心库辅助文档

### core/Accelerator

主要是接口API等

### core/Element

1. `bool LS::Element::intersect(const Ray& ray, Intersection& isect, double* t)`

一个元件由多个更基础的shape组成，调用元件的求交方法后，将会逐个调用改元件下每个shape各自的求交方法。

2. `LS::BoundingBox LS::Element::getWorldBoundingBox()`

获取该元件相对世界坐标系的包围盒。

怎么获取呢？包围盒类中定义了一个extend函数，可以把另一个包围盒和当前的包围盒结合（每个包围盒只用pMin和pMax两个坐标表述）。遍历当前元件中所有的shape并把他们的包围盒结合起来即可。

3. `void LS::Element::setSurface(SurfaceApi* surface, const std::string& surfaceId)`

TODO：

不是很清楚这个函数具体怎么操作设定的surface。。

不过似乎程序并没有调用这个函数。

假设：一个元件（的材质一般是相同的，所以）具有的表面属性是相同的，因此需要把表面属性逐个配置到组成该元件的每个shape中？

1. `void addBind(const std::string& surfaceTypeId, Shape* shape)`

这个函数用来生成/维护surfaceTypeIdToShapes这个表单。根据surfaceTypeId把shape和ID的对应关系添加到surfaceTypeIdToShapes中。

TODO：
但是surfaceTypeIdToShapes这个东西具体是有什么用呢？

### core/Filter

主要是接口API等

### core/Integrator

**class Scece**

PauseTool

使用condition variable中的cv.wait()和cv.notify_all()两个函数来实现线程的启停

SubsetInfo

子任务信息的保存和维护，init函数遍历每个element中的每个shape并将其添加到hitNums中，做初始化

**class Integrator**

1. `LS::Integrator::Integrator(const ExportedObjectInitParas& initParas):ExportedObject(initParas)`

初始化仿真参数数据类 simParas，并且从initParas中解析序列化参数。

之后判断是否是序列化仿真，如果是，则将序列化信息读入sequenceSimInfos。

2. `void collectRay(const Ray& fromSensorRay, RTResult& rtResult, Sensor* sensor, bool checkRestriction = false) const`

将从接受器发射的单根光线的仿真结果添加到接收器的临时结果中。

```cpp
if (checkRestriction && sensor->getRestriction() != nullptr) {
    Intersection tmpIsect;
    //出射光线的原点作为Isect对象的hitpoint
    tmpIsect.pos = fromSensorRay.origin;
    glm::dvec3 normal(0.0, 0.0, 1.0);
    //将法线(0,0,1)根据接收器相对世界坐标的位置坐标变换到世界坐标下
    normal = sensor->localToWorld.forNormal(normal);
    //把转换后的法线传到Isect对象中
    tmpIsect.coords.init(normal);
    //判断当前这根sensorRay是否符合约束
    if (!sensor->getRestriction()->isInRange(tmpIsect, fromSensorRay.direction, sensor)) {
        return;
    }
}
//如果符合约束则保存这根光线。
sensor->addSample(fromSensorRay, rtResult, true);
```

3. `bool collectRay(const Ray& toSensorRay, RTResult& rtResult, SensorCollectInfo& info, Sensor* targetSensor = nullptr) const`

将从光源出发但是会进入到接收器范围内的单根光线的仿真结果添加到接收器的临时结果中。 遍历所有接收器进行判断，若是在其范围内则添加结果。

```cpp
bool show = false;
auto sensors = scene->sensors;
if(targetSensor != nullptr){
    sensors.clear();
    //如果有指定接收器的话，就只考虑这个接收器而不是全部
    sensors.push_back(targetSensor);
}
for (Sensor* sensor : scene->sensors) {
    Intersection isect;
    //遍历所有接收器和当前光线做求交测试，如果有相交
    if (sensor->hitSensor(toSensorRay, isect)) {
        auto localToSensorRay = toSensorRay;
        //体绘制
        localToSensorRay.updateVolumeAbosrbing(isect.pos);
        //更新参数
        sensor->addSample(localToSensorRay, rtResult, false);
        info.sensors.push_back(sensor);
        info.positions.push_back(isect.pos);
        show = true;
    }
}
return show;
```

0729：TTOODDOO:getSubsetInfo,doSim等









