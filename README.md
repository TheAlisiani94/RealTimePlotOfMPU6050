# RealTimePlotOfMPU6050
#Real Time plotting of MPU6050 data 

from mpu6050 import mpu6050
import matplotlib.pyplot as plt
import time

sensor = mpu6050(0x68)  # Initialize the MPU6050 sensor with the correct I2C address

accel_x = []
accel_y = []
accel_z = []

gyro_x = []
gyro_y = []
gyro_z = []

plt.ion()  # Turn on interactive plotting mode

fig, (ax1, ax2) = plt.subplots(2, 1)  # Create two subplots for the accelerometer and gyroscope data

ax1.set_ylim(-20, 20)  # Set the y-axis limits for the accelerometer data
ax1.set_title("Accelerometer Data")
ax1.set_xlabel("Time (s)")
ax1.set_ylabel("Acceleration (m/s^2)")

ax2.set_ylim(-250, 250)  # Set the y-axis limits for the gyroscope data
ax2.set_title("Gyroscope Data")
ax2.set_xlabel("Time (s)")
ax2.set_ylabel("Angular Velocity (deg/s)")

start_time = time.time()  # Get the current time


while True:
    accel_data = sensor.get_accel_data()  # Get the accelerometer data
    gyro_data = sensor.get_gyro_data()  # Get the gyroscope data

    current_time = time.time() - start_time  # Calculate the elapsed time

    accel_x.append(accel_data['x'])
    accel_y.append(accel_data['y'])
    accel_z.append(accel_data['z'])

    gyro_x.append(gyro_data['x'])
    gyro_y.append(gyro_data['y'])
    gyro_z.append(gyro_data['z'])

    ax1.plot(current_time, accel_data['x'], 'ro', label='X-axis')
    ax1.plot(current_time, accel_data['y'], 'go', label='Y-axis')
    ax1.plot(current_time, accel_data['z'], 'bo', label='Z-axis')
   

    ax2.plot(current_time, gyro_data['x'], 'ro', label='X-axis')
    ax2.plot(current_time, gyro_data['y'], 'go', label='Y-axis')
    ax2.plot(current_time, gyro_data['z'], 'bo', label='Z-axis')

    fig.canvas.draw()  # Redraw the plot

    plt.pause(0.001)  # Pause to allow the plot to update
