# Final Thesis

## A Deep Learning Implementation for Inverse Hessian Estimation in Multiscale Full Waveform Inversion

## Research Purposes  
This research aims to:
1. Reconstruct the subsurface velocity model using multiscale Full Waveform Inversion (FWI) with Conjugate Gradient and quasi-Newton (L-BFGS) optimization.  
2. Estimate the inverse Hessian with deep learning methods so that it can be applied to FWI with Gauss–Newton optimization.  
3. Analyze the velocity models produced by Conjugate Gradient, L-BFGS, and Gauss–Newton optimization in terms of resolution and convergence  

## Methods  
### Forward Modeling  
Forward modeling, as described by Virieux and Operto (2009), refers to the process of simulating seismic wave propagation through a given subsurface model using the wave equation. Starting from a defined source function, such as a Ricker wavelet, the wave equation is numerically solved—commonly with finite-difference, finite-element, or spectral-element methods—to generate synthetic seismic data at receiver positions.

<img width="588" height="721" alt="image" src="https://github.com/user-attachments/assets/0df50046-2e57-4331-a95b-06a8ef23cd0e" />

  
Figure 1 Forward modeling illustration  
  
### Adjoint Modeling
Adjoint modeling is the computational process used in seismic inversion (especially Full Waveform Inversion, FWI) to efficiently compute the **gradient** of the **misfit function** with respect to the model parameters through adjoint state function.  
  
### Optimization
1. L-BFGS is a quasi-Newton optimization method that approximates the inverse Hessian matrix using information from a limited number of previous iterations. Unlike full BFGS, which requires storing and updating a dense approximation of the Hessian, L-BFGS maintains only a small history of gradient and parameter updates, making it much more memory efficient.  
2. The Conjugate Gradient method is an iterative optimization algorithm designed to solve large-scale systems of linear equations and unconstrained nonlinear optimization problems without the need to store or explicitly compute the Hessian matrix.  
3. The Gauss–Newton method is an optimization algorithm tailored for solving nonlinear least-squares problems, such as those encountered in seismic Full Waveform Inversion (FWI). It is derived as a simplification of Newton’s method: instead of using the full Hessian matrix of the objective function, which is expensive to compute, Gauss–Newton approximates the Hessian by retaining only the first-order derivative terms (the Jacobian transpose multiplied by the Jacobian and neglecting the second-order terms.
   
### Multiscale FWI  
Multiscale Full Waveform Inversion (FWI) is a strategy designed to mitigate the problem of cycle-skipping, a common issue in seismic inversion where the optimization converges to an incorrect local minimum if the phase difference between observed and synthetic data exceeds half a cycle. The multiscale approach addresses this by progressively inverting the data from low to high frequencies (Bunks, et al., 1995).  

### Deep Learning (UNet)  
The purpose of this Deep Learning phase is to create a better update model through a UNet architecture. According to Ronneberger et al. (2015), the U-Net architecture is a convolutional neural network (CNN) specifically designed for semantic segmentation tasks, originally in the biomedical imaging domain. Its name comes from its U-shaped structure, consisting of two main paths: Encoder and Decoder.  
  
<img width="718" height="480" alt="image" src="https://github.com/user-attachments/assets/b4309151-fb14-4ffd-a9ab-8b773fb3ad0a" />    
  
Figure 2 The UNet Architecture (Ronneberger, et al., 2015)  

In order to generate the training data, Born modeling (demigration) and its adjoint (migration) is proposed as we strongly refer to Alfarhan, et al. 2024. The product of this approach is the blurred version of gradient, meaning that we can put it as the training data and clear gradient that we produce through the adjoint modeling phase as the label data. By using the MSE loss in PyTorch, we can see that the loss should be decaying as the epochs increases. The result of the UNet model is the enhanced update model as UNet is playing as the inverse Hessian.  

<img width="512" height="480" alt="image" src="https://github.com/user-attachments/assets/a21e9866-73e5-4093-a484-7d914d2b9d6d" />  
  
Figure 3 The design of the deep learning implementation. similar to the workflow proposed by Alfarhan, et al., 2024.  

## Result
<img width="715" height="575" alt="image" src="https://github.com/user-attachments/assets/11adf57e-f848-405b-8623-cdc4e6af9892" />    
  
*Title for the third model should be "Final Model GN-UNet" instead of "Final Model Newton (UNet)"  
Figure 4 The final result of the method (L-BFGS, Conjugate Gradient, GN-UNet)  

<img width="407" height="327" alt="image" src="https://github.com/user-attachments/assets/adaaf4b7-30d3-4e29-b97c-dde968853632" />  
  
Figure 5 MSE and MAE loss over 3 different scale (30 simulations each scale [5 Hz, 10 Hz, and 25Hz cutoff])  

<img width="836" height="408" alt="image" src="https://github.com/user-attachments/assets/e3e042eb-9af3-478c-bfee-415b52f76b6d" />
  
Figure 6 Data loss evolution througout the simulations. + UNet loss in first FWI iteration (50 epochs)  

## Key Takeaways  
Thus, conclusions were drawn that address the research objectives presented at the beginning. The conclusions are as follows:  
1. The reconstruction of the model can be carried out using the L-BFGS and Conjugate Gradient optimization methods, as demonstrated by the obtained and analyzed results.
2. The UNet architecture in deep learning has been shown to estimate the inverse Hessian in the case of FWI, although further research is needed to achieve more ideal results.
3. The velocity model obtained with the Newton Method (UNet) exhibits the best resolution, as indicated by the MSE and MAE analyses.

## Further Suggestions  
By considering the potential of applying deep learning (DL) to the FWI case, the following suggestions are proposed for further research on related topics:  
1. The application of alternative methods to estimate the second-order term of the Hessian in Newton’s method in order to obtain results more consistent with the full Newton approach.
2. The application of other DL architectures (e.g., FCNN, GAN, etc.) to produce better estimations than those obtained using the UNet architecture.

## References  
Alfarhan, M., Ravasi, M., Chen, F., dan Alkhalifah, T. (2024). Robust Full Waveform Inversion with deep Hessian deblurring. https://doi.org/10.1093/gji/ggae378   
Bunks, C., Saleck, F. M., Zaleski, S., dan Chavent, G. (t.t.). Multiscale seismic waveform inversion, GEOPHYSICS, diperoleh melalui situs internet: http://library.seg.org/, 60.  
Kumar, A., Hampson, G., Rayment, T., dan Burgess, T. (t.t.). A DEEP LEARNING INVERSE HESSIAN FOR LEAST-SQUARES MIGRATION.  
Louboutin, M., Witte, P., Lange, M., Kukreja, N., Luporini, F., Gorman, G., dan Herrmann, F. J. (2017). Full-waveform inversion, Part 1: Forward modeling, The Leading Edge, 36(12), 1033–1036. https://doi.org/10.1190/tle36121033.1  
Métivier, L., Bretaudeau, F., Brossier, R., Operto, S., dan Virieux, J. (2014). Full waveform inversion and the truncated Newton method: quantitative imaging of complex subsurface structures, Geophysical Prospecting, 62(6), 1353– 1375. https://doi.org/10.1111/1365-2478.12136  
Nocedal, J., dan Wright, S. J. (2006). Numerical Optimization, Springer New York. https://doi.org/10.1007/978-0-387-40065-5  
Schuster, G. T. (2017). Seismic Inversion, Society of Exploration Geophysicists. https://doi.org/10.1190/1.9781560803423  
Schuster, G. T., Chen, Y., Shi, Y., Feng, S., Liu, Z., Feng, Z., Huang, Y., Yang, Y., dan Gautam, T. (2022). Machine Learning Methods in Geoscience.  
Tarantola, A. (1984). Inversion of seismic reflection data in the acoustic approximation, GEOPHYSICS, 49(8), 1259–1266. https://doi.org/10.1190/1.1441754  
Virieux, J., dan Operto, S. (2009). An overview of full-waveform inversion in exploration geophysics, GEOPHYSICS, 74(6), WCC1–WCC26. https://doi.org/10.1190/1.3238367  
Wang, Z., Bovik, A. C., Sheikh, H. R., dan Simoncelli, E. P. (2004). Image quality assessment: From error visibility to structural similarity, IEEE Transactions  

Throughout the filtering phase, I used https://github.com/mrava87/Devito-fwi as the reference since his code are easy to comprehend  
