Train.py

1.初始化

```python
    # Setup random seed
    # 设定随机种子

    torch.manual_seed(config['seed'])

    np.random.seed(config['seed'])

    random.seed(config['seed'])

    torch.cuda.manual_seed(config['seed'])

  

    # create experience name
    # 创建实验名称

    curr_date = datetime.now()

    exp_name = config['name'] + '___' + curr_date.strftime('%b-%d-%Y___%H-%M-%S')

    print(exp_name)

  

    # Create directory structure
    # 创建outputfile

    output_folder = Path(config['output']['dir'])

    output_folder.mkdir(parents=True, exist_ok=True)

    (output_folder / exp_name).mkdir(parents=True, exist_ok=True)

    # and copy the config file
    # 将config.json写入output文件

    with open(output_folder / exp_name / 'config.json', 'w') as outfile:

        json.dump(config, outfile)

  

    # set device
    # 设置cuda

    device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')

  

    # Initialize tensorboard
    # 初始化Tensorboard

    writer = SummaryWriter(output_folder / exp_name)
```

2.[[加载数据集]]
```python
    # Load the dataset

    enc = ra_encoder(geometry = config['dataset']['geometry'],

                        statistics = config['dataset']['statistics'],

                        regression_layer = 2)


    dataset = RADIal(root_dir = config['dataset']['root_dir'],

                        statistics= config['dataset']['statistics'],

                        encoder=enc.encode,

                        difficult=True)

  

    train_loader, val_loader, test_loader = CreateDataLoaders(dataset,config['dataloader'],config['seed'])
```


3.初始化[[模型]]
```python
    # Create the model

    net = FFTRadNet(blocks = config['model']['backbone_block'],

                        mimo_layer  = config['model']['MIMO_output'],

                        channels = config['model']['channels'],

                        regression_layer = 2,

                        detection_head = config['model']['DetectionHead'],

                        segmentation_head = config['model']['SegmentationHead'])


    total = sum([param.nelement() for param in net.parameters()])

    print("Number of parameter: %.2fM" % (total/1e6))

	net.to("cuda")
```

4.初始化优化器
```python
    # Optimizer

    lr = float(config['optimizer']['lr'])
    # "lr": 1e-4

    step_size = int(config['lr_scheduler']['step_size'])
    # "step_size": 10

    gamma = float(config['lr_scheduler']['gamma'])
    # "gamma": 0.9

    optimizer = optim.Adam(filter(lambda p: p.requires_grad, net.parameters()), lr=lr)

    scheduler = lr_scheduler.StepLR(optimizer, step_size=step_size, gamma=gamma)

  

    num_epochs=int(config['num_epochs'])

  
  

    print('===========  Optimizer  ==================:')

    print('      LR:', lr)

    print('      step_size:', step_size)

    print('      gamma:', gamma)

    print('      num_epochs:', num_epochs)

    print('')
```

5.开始训练
```python
    # Train

    startEpoch = 0

    global_step = 0

    history = {'train_loss':[],'val_loss':[],'lr':[],'mAP':[],'mAR':[],'mIoU':[]}

    best_mAP = 0

  

    freespace_loss = nn.BCEWithLogitsLoss(reduction='mean')



    for epoch in range(startEpoch,num_epochs):

        kbar = pkbar.Kbar(target=len(train_loader), epoch=epoch, num_epochs=num_epochs, width=20, always_stateful=False)

        ###################

        ## Training loop ##

        ###################

        net.train()

        running_loss = 0.0

        for i, data in enumerate(train_loader):

            # inputs = [data[0].detach().to('cuda').float(), data[4].detach().to('cuda').float(), data[5].to('cuda').float()]

            inputs = [data[0].detach().to('cuda').float(), torch.stack(data[3]).detach().to('cuda').float(), data[5].to('cuda').float()]

#            inputs.append(data[0].to('cuda').float())

            # FFT:4x32x512x256

#            inputs.append(data[4].to('cuda').float())

            # Image:4x540x960x3

#            inputs.append(data[5].to('cuda').float())

            # pcl:4xkx4

            label_map = data[1].to('cuda').float()

            # 4x3x128x224

            if(config['model']['SegmentationHead']=='True'):

                seg_map_label = data[2].to('cuda').double()

                # 4x256x224

  

            # reset the gradient

            optimizer.zero_grad()

            # forward pass, enable to track our gradient

            # with torch.set_grad_enabled(True):

            #     outputs = net(inputs)

            outputs,RD_feature,RA = net(inputs)

            # Image_feature = make_grid(Image_feature[0].detach().cpu().unsqueeze(dim=1), padding=20, normalize=True, scale_each=True, pad_value=1)

            RD_feature = make_grid(RD_feature[0].detach().cpu().unsqueeze(dim=1), padding=20, normalize=True, scale_each=True, pad_value=1)

            RA = make_grid(RA[0].detach().cpu().unsqueeze(dim=1), padding=20, normalize=True, scale_each=True, pad_value=1)

            # writer.add_image('Image_feature_map', Image_feature, global_step)

            writer.add_image('RD_feature_map', RD_feature, global_step)

            writer.add_image('RA', RA, global_step)
```
