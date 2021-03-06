# Shape Matters: Understanding the Implicit Bias of the Noise Covariance

Code for the paper "Shape Matters: Understanding the Implicit Bias of the Noise Covariance". The original paper is [here](https://arxiv.org/abs/2006.08680).

## Experiments for VGG19 model on Cifar100
The flag ``--update_type`` determines which update to use: standard SGD, adding Gaussian noise, or adding label noise. The flag ``--label_noise`` determines the scale of the label noise that is added, and the flag ``--inner_lr`` determines the scale of Gaussian noise added. All scripts should be run in the folder ``cifar100``.

Small batch baseline:

`python train_cifar.py --arch vgg19 --lr_sched vgg_default --update_type standard --lr 0.004 --iters 410550 --train_by_iters --batch_size 26 --iters_per_epoch 1173 --dataset cifar100 --weight_decay 0 --data_dir <PATH TO DATA>`

Large batch baseline:

`python train_cifar.py --arch vgg19 --lr_sched vgg_default --update_type standard --lr 0.004 --iters 410550 --train_by_iters --batch_size 256 --iters_per_epoch 1173 --dataset cifar100 --weight_decay 0 --data_dir <PATH TO DATA>`

To run with label noise:

`python train_cifar.py --arch vgg19 --lr_sched vgg_default --update_type mean_zero_label_noise --lr 0.004 --iters 410550 --train_by_iters --batch_size 256 --iters_per_epoch 1173 --dataset cifar100 --label_noise 0.1 --ln_sched vgg_default --ln_decay 0.5 --also_flip_labels --weight_decay 0 --data_dir <PATH TO DATA>`

To run with Gaussian noise with sigma = 7.5e-5:

`python train_cifar.py --arch vgg19 --lr_sched vgg_default --update_type gaussian_drift --lr 0.004 --iters 410550 --train_by_iters --batch_size 256 --iters_per_epoch 1173 --dataset cifar100 --inner_lr 0.000075 --inner_anneal vgg_default --weight_decay 0 --data_dir <PATH TO DATA>`


## Experiments for the quadratically-parameterized model

The flag ``--d`` determines the dimension of the model, ``--m`` determines the number of random data, ``--rho`` determines the sparsity of ground truth, ``--alpha`` determines the scale of initialization, ``--opt`` determines the type of noise added to full batch gradient in each iteration, ``--delta`` determines the scale of noise, ``--bs`` determines the batch size that is used to calculate the mini-batch noise. All scripts should be run in the folder ``quadratic``.

Full batch baseline:

`python train_quadratic.py --opt=GD --lr=0.01 --d=100 --rho=5 --m=40 --iter=300000 --alpha=1.0 --log_interval=20`

Gradient descent with mini-batch noise baseline:

`python train_quadratic.py --opt=SGD --lr=0.01 --d=100 --rho=5 --m=40 --iter=300000 --delta=1.0 --bs=1 --alpha=1.0 --log_interval=20`

Gradient descent with label noise baseline:

`python train_quadratic.py --opt=LNGD --lr=0.01 --d=100 --rho=5 --m=40 --iter=300000 --delta=1.0 --alpha=1.0 --log_interval=20`

Gradient descent with Gaussian noise with sigma = 1e-2:

`python train_quadratic.py --opt=SGLD --lr=0.01 --d=100 --rho=5 --m=40 --iter=1200000 --delta=0.01 --alpha=1.0 --log_interval=20`

