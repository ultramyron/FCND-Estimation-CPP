# Estimator Project

### 1. Determining standard deviation of measurement noise
To determine the standard deviation, the text file was imported into excel and the standard deviation of the GPS and accelerometer data was found using the STDEV function. 

### 2. Attitude Estimation

The attitude estimation implementation can be found in ***lines 96-120***. A rotation matrix was constructed to transform the gyroscope measurements (p,q,r) into the euler angle derivatives. After that, the derivatives were integrated to find the change in roll, pitch and yaw. This integration was then added to the current roll, pitch, and yaw to find the predicted state.  The yaw angle was then normalized to find the smallest angle.

### 3. Predicting the State
The function to predict the state forward in time is implemented in ***lines 184-190***. The state was propagated forward using the derivatives of each state vector, where I used an Euler forward integration scheme. The acceleration had to be converted from the body frame to the inertial frame in order to propagate the state properly.
The function to predict the covariance forward in time is implemented in ***lines 270-276***. To do this, the derivative of the transition function and the body to global frame rotation matrix derivative must be calculated.
Transition Function Derivative:

![enter image description here](https://lh3.googleusercontent.com/y5gc5OqwT67tvoyuia0RNPPKeBgD5Nnm6cEY1NqB-iBRqTmhahPj0335W51oR67oVHQeXayOCIkRgCAJx628DAhTWVHql0E9dGSyD3naT8QAJKrDb6t2tMMrB4y7R5mORh7Jv5JpeA2tRXN3F8jsam19cQAo5FSGqq1s8DHM_MpSmlajydx7qJE1H3aLjivgNObeS5HVVD42Y1pU7t80fgthLkV5aiDq-2wwQ-ccYjVYZtA6tumRPv5mxQp5L2ZE4hRFJp3lk4IKr45-fhohTVYkYeMGDOCQRVdK04e0AYSDGGQKjM-uYCzhnwBcqMJkeCndZUP2u1hKwJN5N6-HMpl9J6-VqueuqVKnjPlJNZ-B70msygkuB0UHO7Bywd5gWjgAEjsW5ieyhIY0XkqNxw8-GXDDHnIuBjX43jT6LKEPYsEKCGb4e0vL8kFDKU1qUHllf8N7MdIB5wWcKrTK8BrYcyxheq3p-fHKPV3dxPrTLr0g7lmkWTNPp-FSlMLW7iqLcpiesS3Q6tDgam6VhxTeaiSAPHGrvG0B1mWoc5sMi-ty2_V1uMIlUUd2tpLLOX2KN69dSPEeRW0azkQc5VuxRi_8vWViA4zU48zuQQ_Zp7zfvWBkZ1O6p3heiqucRWinMUK0S07_02zzYn0Ukj3TG0bqalxZTiF21saMoL_BPO7ozfna5jI1gxAvOVtzFNH9vA0Q48wObkzBLQ2PuvY=w716-h312-no?authuser=0)

Rotation Matrix Derivative (implementation found in ***lines 217-225***):

![enter image description here](https://lh3.googleusercontent.com/T-_Kk6E0igT2Z-RJPijNd93eF6k11RLZlkAw4BLmeEmzK_Eh5CNLIga3woGEL44SEzHCQp0ADz5r1wSQmemmz9ZERRtHKfqGVChBIXpFMmJ3OWO_Ezzk2kzWF5eZ9UI8CsgJng_gxa-WeM2OGag4pnnz3mUnO8nDx-FHnBxmBALSk1EZRzDMc89LMDHMi9xnCVqTvQtqHm5T--vucZO41YhZ3dxcYwvVNnFFlp4gtCjbIKU7wmyg3tbVi9l1ACBnwKRv0RvvJSxUMGs6WfmTcUKa3938o81OrifLrYiK-vD27C3SvRyD7cb1arAk566Q40Y8fpCrAEIpjyH4gTGGewyXQhywZhzOjQR2Syx5YsilR_yepfgxX0zo2jR8xakpWiJw4I1eq97pDaJX91_te1W75VBZcshyPgzE26C9XGmSlsLHlogxCst2yalR_426zlxZGfzYZNchHbIum-tHqDU4lyZH4ZsRJn2mKMCEFKSbUoLEvt5uef33vOA6aQddBhHwknhH1UFqcMdPgZZieb2wFuN0ngqn4m0LJX2mG8V2WKlK_ORA7K83GCCMu1BM3ArY-gWlaLLK21MZm_bGi9mryVIms9fed4M5mB4KS1NsD4zw455PYOq9bNFXq_P9Lf9NgHv4b2NlOOWf5eM9qTNHVjRZLXSFYGckGIIK_HLY6eNbfLl4jWrK8rHFjqVXnmz61m-iAzIkNG2lVUy__O4=w1316-h180-no?authuser=0)

With these matrices, along with the transition model covariance, and updated state variance can be calculated.

### 4. Magnetometer Update
The magnetometer update function implementation can be found in ***lines 332-341***. This function simply took the difference between the measured magnetometer reading and the predicted magnetometer reading and normalized the predicted reading so that we would update the reading using the short angle, not the long angle.

The predicted readings and measurement function are then supplied to the update function to propagate the new state and covariance.

### 5. GPS Update
The GPS update function implementation can be found in ***lines 301-309***. This function updates the state variable from the prediction step and creates the H' matrix. The new state and H' matrix are then sent to the update function to retrieve a new mean and covariance.
