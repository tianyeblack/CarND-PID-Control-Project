### Command to Run
./pid 0.2 0.0000009 12.8 or ./pid (There are default params if not specified)

### PID Coefficients
#### Effects
Coefficient P has more direct influence on the resulting steering value. If too small, following sharp turns will be challenging as there is not enough steering. However, a very large value will cause the vehicle to swing too much. This is because even a small CTE can create a large steering value.

Coefficient D acts as a damping factor if the vehicle swings left and right too much but strengthens turning if the steering value was insufficient to reduce CTE. The actual effect also depends on the speed of the vehicle.

Coefficient I corrects systematic bias of the vehicle. If the vehicle always turns less than desired (CTE not decreasing), this coefficient will contribute more and more to the steering value and eventually make it large enough.
#### Tuning

I tuned the params by hand, starting from P then D and finally I.

First, I experimented with different P values, and settled down around 0.1 to 0.2. Larger values tend to make the vehicle swings wider and wider as the speed increases and it enters the first turn. Smaller values produces in sufficient turning at the 2nd and 3rd turn.

Then, I started experimenting with D values around the same magnitude as P. The vehicle ends up swinging about as much as if there is no D coefficient. After gradually increasing it to around 50x to 100x of P. The vehicle would be able to make through all turns but begin to swing a lot near the starting point. Making it even larger creates an interesting phenomenon: the vehicle would turn very hard when entering to correct the track but then fail to maintain that and often rush out of the track. One explanation for this is: at turns, when entering, the vehicle have increasing CTE for a short period, then turns really hard with P and large D coefficients to correct it quickly; it would unavoidably reach the other side of the center line and has a steering value opposite of the turn; finally due to the high speed, it would rush out of the track before the algorithm corrects it.

Lastly, I added coefficient I to experiments. I also picked a value around the same magnitude as P to begin with, which turned out so bad the vehicle ended up just turning really hard and not moving. Due to the fact this is an integral factor and the vehicle starts slightly off center, the error term accumulates very fast. In the end, I chose 0.0000009 as anything larger would at a certain point generate large error term and the vehicle goes off track.

On top of tuning these coefficients, I also added damping to throttle, which decreases throttle when steering value is large. This slows down vehicle at turns and during any side-to-side swings, which is what humans naturally do when they realize the vehicle is harder to control.  