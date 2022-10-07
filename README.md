

import numpy as np

import gspread

gc = gspread.service_account(filename='unitydatasciense-364213-20e2004004c6.json')

sh = gc.open("UnitySheets")

# define data, and change list to array

x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]

x = np.array(x)

y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]

y = np.array(y)


# The basic linear regression model i wx+ b, and since this is a two-dimensional spase, the model is ax+ b

def model(a, b, x):

    return a * x + b


# The most commonly used loss function of liner regression model is the loss function of mean variance difference
def loss_function(a, b, x, y):

    num = len(x)
    
    prediction = model(a, b, x)
    
    return (0.5 / num) * (np.square(prediction)).sum()


# The optimization function mainly USES partial derivaties to update two parameters a and b
def optimize(a, b, x, y):

    num = len(x)
    
    prediction = model(a, b, x)
    
    # Update the values of A and B by finding the partial derivaties of loss the function on a and b
    
    da = (1.0 / num) * ((prediction - y) * x).sum()
    
    db = (1.0 / num) * (prediction - y).sum()
    
    a = a - Lr * da
    
    b = b - Lr * db
    
    return a, b


# iterated function, return a and b

def iterate(a, b, x, y, times):

    for i in range(times):
    
        a, b = optimize(a, b, x, y)
        
    return a, b


# Initialize parameters and display

a = np.random.rand(1)

print(a)

sh.sheet1.update('A1', 'a')

sh.sheet1.update('B1', str(a))

b = np.random.rand(1)

print(b)

sh.sheet1.update('A2', 'b')

sh.sheet1.update('B2', str(b))

Lr = 0.000001

a, b = iterate(a, b, x, y, 1)

prediction = model(a, b, x)

loss = loss_function(a, b, x, y)
print(a, b, loss)

#plt.scatter(x, y)

#plt.plot(x, prediction)

sh.sheet1.update('A3', str(a))

sh.sheet1.update('B3', str(b))

sh.sheet1.update('C3', str(loss))
