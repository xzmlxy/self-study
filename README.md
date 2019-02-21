
https://www.codetd.com/article/1657882


# -*- coding: utf-8 -*-
"""
Created on Sat Jun 17 17:44:42 2017

@author: 代码医生 qq群：40016981，公众号：xiangyuejiqiren
@blog：http://blog.csdn.net/lijin6249
"""
import tensorflow as tf
import numpy as np

# 网络结构：2维输入 --> 2维隐藏层 --> 1维输出

learning_rate = 1e-4
n_input = 2
n_label = 1
n_hidden = 2

x = tf.placeholder(tf.float32, [None, n_input])
y = tf.placeholder(tf.float32, [None, n_label])

weights = {
    'h1': tf.Variable(tf.truncated_normal([n_input, n_hidden], stddev=0.1)),
    'h2': tf.Variable(tf.random_normal([n_hidden, n_label], stddev=0.1))
}
biases = {
    'h1': tf.Variable(tf.zeros([n_hidden])),
    'h2': tf.Variable(tf.zeros([n_label]))
}

layer1 = tf.add(tf.matmul(x, weights['h1']), biases['h1'])
layer_1 = tf.maximum(layer1,0.01 * layer1)
#layer_1 = tf.sigmoid(layer1)
layer2 = tf.add(tf.matmul(layer_1, weights['h2']), biases['h2'])
# y_pred = tf.nn.tanh(layer2)
# y_pred = tf.nn.relu(layer2)
y_pred = tf.nn.sigmoid(layer2)
# Leaky relus
# y_pred = tf.maximum(layer2, 0.01 * layer2)

loss = tf.reduce_mean((y_pred - y) ** 2)
train_step = tf.train.AdamOptimizer(learning_rate).minimize(loss)

# 生成数据
X = [[0, 0], [0, 1], [1, 0], [1, 1]]
Y = [[0], [1], [1], [0]]

sess = tf.InteractiveSession()
train_times = 100
time = 1
while time < train_times:
    sess.run(tf.global_variables_initializer())
    for i in range(40000):
        _, loss_1 = sess.run([train_step, loss], feed_dict={x: X, y: Y})
        if i % 1000 == 0:
            print("time = " + str(time) + ";loss = " + str(loss_1))
    # print(weights['h1'].eval())
    # print(biases['h1'].eval())
    # print(weights['h2'].eval())
    # print(biases['h2'].eval())
    # print(sess.run(tf.add(tf.matmul(x, weights['h1']), biases['h1']), feed_dict={x: X}))
    # print("layer1")
    # print(sess.run(layer_1, feed_dict={x: X}))
    # print(sess.run(layer2, feed_dict={x: X}))
    if loss_1 <= 0.01:
        break
    else:
        time += 1
# 计算预测值
print(sess.run(y_pred, feed_dict={x: X}))
# 输出：已训练100000次
