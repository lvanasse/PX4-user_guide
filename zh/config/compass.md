# 罗盘校准

罗盘校准过程配置了所有连接的外部和内部 [magnetometers](../gps_compass/README.md)。 *QGroundControl* will guide you to position the vehicle in a number of set orientations and rotate the vehicle about the specified axis.

## Overview

You will need to calibrate your compass on first use, and you may need to recalibrate it if the vehicles is ever exposed to a very strong magnetic field, or if it is used in an area with abnormal magnetic characteristics.

:::tip
Indications of a poor compass calibration include multicopter circling during hover, toilet bowling (circling at increasing radius/spiraling-out, usually constant altitude, leading to fly-way), or veering off-path when attempting to fly straight. _QGroundControl_ should also notify the error `mag sensors inconsistent`.
:::

Two types of compass calibration are available:

1. [Complete](#complete-calibration): This calibration is required after installing the autopilot on an airframe for the first time or when the configuration of the vehicle has changed significantly. It compensates for hard and soft iron effects by estimating an offset and a scale factor for each axis.
1. [Partial](#partial-quick-calibration) ("Quick Calibration"): This calibration can be performed as a routine when preparing the vehicle for a flight, after changing the payload, or simply when the compass rose seems inaccurate. This type of calibration only estimates the offsets to compensate for a hard iron effect.


## 执行校准

### Preconditions

Before starting the calibration:

1. Choose a location away from large metal objects or magnetic fields. :::tip
Metal is not always obvious! Avoid calibrating on top of an office table (often contain metal bars) or next to a vehicle. 
Calibration can even be affected if you're standing on a slab of concrete with uneven distribution of re-bar.
:::
1. Connect via telemetry radio rather than USB if at all possible. USB can potentially cause significant magnetic interference.
1. If using an external compass (or a combined GPS/compass module), make sure it is [mounted](../assembly/mount_gps_compass.md) as far as possible from other electronics in order to reduce magnetic interference, and in a _supported orientation_.

### Complete Calibration

The calibration steps are:

1. Start *QGroundControl* and connect the vehicle.
1. Select the **Gear** icon (Vehicle Setup) in the top toolbar and then **Sensors** in the sidebar.
1. Click the **Compass** sensor button.

   ![选择 Compass 校准 PX4](../../assets/qgc/setup/sensor/sensor_compass_select_px4.jpg)

:::note
You should already have set the [Autopilot Orientation](../config/flight_controller_orientation.md). If not, you can also set it here.
:::
1. Click **OK** to start the calibration.
1. 把你的飞机放置在下面显示的某一个方向，并保持静止。 随后提示（方向图像变为黄色）在指定方向旋转飞行器。 该位置标定完成后，屏幕上的相应图示将变成绿色。

   ![PX4 上的罗盘校准步骤](../../assets/qgc/setup/sensor/sensor_compass_calibrate_px4.jpg)

1. 在机体的所有方向上重复校准步骤。

Once you've calibrated the vehicle in all the positions *QGroundControl* will display *Calibration complete* (all orientation images will be displayed in green and the progress bar will fill completely). You can then proceed to the next sensor.

### Partial "Quick" Calibration

This calibration is similar to the well-known figure-8 compass calibration done on a smartphone:

1. Hold the vehicle in front of you and randomly perform partial rotations on all its axes. 2-3 oscillations of ~30 degrees in every direction is usually sufficient.
1. Wait for the heading estimate to stabilize and verify that the compass rose is pointing to the correct direction (this can take a couple of seconds).

Notes:

- There is no start/stop for this type of calibration (the algorithm runs continuously when the vehicle is disarmed).
- The calibration is immediately applied to the data (no reboot is required) but is saved to the calibration parameters after disarming the vehicle only (the calibration is lost if no arming/disarming sequence is performed between calibration and shutdown).
- The amplitude and the speed of the partial rotations done in step 1 can affect the calibration quality. Following the advice above is ususally enough.

## Additional Calibration/Configuration

The process above will autodetect, [set default rotations](../advanced_config/parameter_reference.md#SENS_MAG_AUTOROT), calibrate, and prioritise, all connected magnetometers.

Further compass configuration should generally not be required.

:::note
All external compasses are given the same priority by default, which is higher than the priority shared by all internal compasses.
:::

### Disable a Compass

As stated above, generally no further configuration should be required.

That said, developers can disable internal compasses if desired using the compass parameters. These are prefixed with [CAL_MAGx_](../advanced_config/parameter_reference.md#CAL_MAG0_ID) (where `x=0-3`).

To disable an internal compass:

- Use [CAL_MAGn_ROT](../advanced_config/parameter_reference.md#CAL_MAG0_ROT) to determine which compasses are internal. A compass is internal if `CAL_MAGn_ROT==1`.
- Then use [CAL_MAGx_PRIO](../advanced_config/parameter_reference.md#CAL_MAG0_PRIO) to disable the compass. This can also be used to change the relative priority of a compass.

## Debugging

Raw comparison data for magnetometers (in fact, for all sensors) can be logged by setting [SDLOG_MODE=1](../advanced_config/parameter_reference.md#SDLOG_MODE) and [SDLOG_PROFILE=64](../advanced_config/parameter_reference.md#SDLOG_PROFILE). See [Logging](../dev_log/logging.md) for more information.

## 更多信息：

- [Peripherals > GPS & Compass](../gps_compass/README.md)
- [Basic Assembly](../assembly/README.md) (setup guides for each flight controller)
- [Compass Power Compensation](../advanced_config/compass_power_compensation.md) (Advanced Configuration)
- [QGroundControl User Guide > Sensors](https://docs.qgroundcontrol.com/master/en/SetupView/sensors_px4.html#compass)
- [PX4 Setup Video - @2m38s](https://youtu.be/91VGmdSlbo4?t=2m38s) (Youtube)

