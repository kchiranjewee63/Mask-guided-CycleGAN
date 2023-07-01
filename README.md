# Mask-Guided CycleGAN

This repository is a modified version of the (pytorch-CycleGAN-and-pix2pix)[https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix], introducing a new component - mask loss. This new loss component guides the CycleGAN's image translation process by focusing on regions defined by a segmentation mask.
Loss Functions:

In this modification, we introduced two extra loss terms: Forward Masked Loss and Backward Masked Loss. The total set of loss functions are:

    Discriminators: D_A: G_A(A) vs. B; D_B: G_B(B) vs. A.
    Forward Cycle Loss: lambda_A * ||G_B(G_A(A)) - A||
    Backward Cycle Loss: lambda_B * ||G_A(G_B(B)) - B||
    Identity Loss: lambda_identity * (||G_A(B) - B|| * lambda_B + ||G_B(A) - A|| * lambda_A)
    Forward Masked Loss: lambda_mask_A * ||G_B(G_A(A)) - A|| * A_mask (New loss term)
    Backward Masked Loss: lambda_mask_B * ||G_A(G_B(B)) - B|| * B_mask (New loss term)

Usage:

The usage remains similar to the original (pytorch-CycleGAN-and-pix2pix)[https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix]. However, if you wish to train with the new mask loss feature, you will need to perform the following steps:

    Add a maskA and/or maskB directory inside the dataset folder. Each directory should contain the corresponding binary mask images.
    Enable the mask loss feature(s) when calling train.py. Pass the following arguments:

```
--mask_loss_A (default False)
--lambda_mask_A (default 10)
--mask_loss_B (default False)
--lambda_mask_B (default 10)
```

If you only have the mask for one type of image (A or B), you can still utilize the mask loss for the corresponding direction. For instance, if only A's masks are available, enable the Forward Masked Loss as follows:

    Add a maskA directory inside the dataset folder with the corresponding mask images.
    Enable the Forward Masked Loss when calling train.py with the following arguments:

```
--mask_loss_A
--lambda_mask_A 10
```

Please replace 10 with your chosen weight for the mask loss.

Introducing this new loss term can help guide the CycleGAN to pay more attention to specific regions of the image during the translation process, potentially improving the quality of the results in these areas. This can be useful in image segmentation problems.