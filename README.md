# 🚘 Lane Following 🚘


### 🧡 Project Purpose

- I participated in a project to develop a lane-following algorithm that allows an autonomous vehicle to reliably detect lanes and drive safely in shaded areas. I was responsible for designing the logic to process sensor data and developing control, decision making algorithms.
- This system was designed for competition purposes, with a focus on determining the driving path and making decisions about the vehicle's behavior based on sensor data. </br>

</br></br>

### 📢 NOTICE

Due to team project policies, the code and details cannot be made public.
</br></br>

### 💯 Performance
![lane](https://github.com/user-attachments/assets/cb52fe7e-9532-494b-95c5-f6bc826b91b7)

  

### ✔ Technical Details
#### Perception
  Using the trained model, lanes are detected, and the center of the lane is calculated and sent to the control decision module in the form of a list
</br></br>
#### Decision making & Control
  - **Vehicle Offset Calculation**</br>
      The current position of the vehicle, received through the camera feed, and the lane center offset are calculated to correct the vehicle's position.</br>
      ```
      # Parts of Codes
      # PD Controller
      def pd_control(self, cte):
              self.d_error = cte - self.p_error
              self.p_error = cte
              self.angle = self.kp * self.p_error +  self.kd * self.d_error
              return self.angle
      ```
      
  - **Steering Angle Calculation Based on PD Control**</br>
      Based on the calculated offset, a PD controller is used to determine the optimal steering angle through experimental tuning to drive the vehicle.</br>
      ```
      # Parts of Codes
      # Calculate Offsets
      rospy.Subscriber('/midpoints', midpoints, self.midpoints_callback, queue_size=10)
      ...
      def midpoints_callback(self, msg):
          if msg.mid_points:  # midpoints가 존재하면 처리
              self.center_x = msg.mid_points[-2]  # 마지막 좌표의 x 값을 가져옴
              rospy.loginfo(f"Received center coordinate: {self.center_x}")
      ```
  - **Control Command Generation and Publishing**</br>
      Generate and publish control commands for steering, acceleration, and braking to the vehicle.</br>
      ```
      # Parts of Codes
      # Calculate Steering & Tuning
      steering_angle = self.pd_control(vehicle_offset)  # PD 제어기
      steering_angle = 0.001 * steering_angle # 0.001은 조향각을 조절해주기 위한 튜닝 파라미터
      steering_angle = steering_angle * 71 # 각도 보정
      ```
</br>
</p>
