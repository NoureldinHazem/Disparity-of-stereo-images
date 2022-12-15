# Disparity-of-stereo-images-
Calculating horizontal disparity using block matching and dynamic programming
<br><br>
Implementing and testing some simple stereo algorithms. <br>
In each case you will take two images Il and Ir (a left and a right image) and compute the horizontal disparity (ie., shift) of pixels along each scanline. This is the so-called baseline stereo case, where the images are taken with a forward-facing camera, and the translation between cameras is along the horizontal axis. We will calculate the disparity using two ways.
<br><br>
1st way: (Block Matching) <br>
The first way to get the disparity is by matching each pixel in the left image to a pixel in the right image. Since there is no rectification needed, you will only need to match the row in the left image with its equivalent in the right image. you will calculate the disparity in two ways,  once using the cost as the Sum of Absolute Differences (SAD) and another time using Sum of Squared Differences. Do this for windows of size w where w = 1, 5 and 9. You will produce 6 maps: 2 maps for each window size, once using SAD and the other using SSD. <br><br>
2nd way: (Dynamic Programming) <br>
The second way to calculate disparity is to use a dynamic programming algorithm to get theminimum cost of matching a whole row in the image. Consider two scanlines Il(i) and Ir(j). Pixels in each scanline may be matched, or skipped (considered to be occluded in either the left or right image). Let dij be the cost associated with matching pixel Il(i) with pixel Ir(j).  Here we consider a squared error measure between pixels given by: <br>
  ![image](https://user-images.githubusercontent.com/89601737/207893132-29d50ea2-e0fe-48e4-9344-5e3281d0c3e0.png)
<br>
where σ is some measure of pixel noise. The cost of skipping a pixel (in either scanline)
is given by a constant c0 . For the experiments here we will use σ = 2 and c0 = 1. Given
these costs, we can compute the optimal (minimal cost) alignment of two scanlines recursively
as follows:
1. D(1, 1) = d11
2. D(i, j) = min(D(i − 1, j − 1) + dij , D(i − 1, j) + c0, D(i, j − 1) + c0)
<br>
The intermediate values are stored in an N-by-N matrix, D. The total cost of matching two scanlines is D(N, N ). Note that this assumes the lines are matched at both ends (and hence have zero disparity there). This is a reasonable approximation provided the images are large relative to the disparity shift. You can find the disparity map of matching the left image to the right or vice versa at the same time or only calculate one of them. Given the cost matrix D we find the optimal alignment by backtracking. In particular, starting at (i, j) = (N, N), we choose the minimum value of D from (i - 1, j - 1), (i - 1, j), (i, j - 1). Selecting (i - 1, j) corresponds to skipping a pixel in Il , so the left disparity map of i is zero. Selecting (i, j - 1) corresponds to skipping a pixel in Ir, and the right disparity map of j is zero. Selecting (i - 1, j - 1) matches pixels (i, j), and therefore both disparity maps at this position are set to the absolute difference between i and j.

<br><br>
Bonus: <br>
A good way to interpret your solution is to plot the alignment found for single scan line. Display the alignment by plotting a graph of Il (vertical) vs Ir (horizontal). Begin at D(N, N ) and  work backwards to find the best path. If a pixel in Il is skipped, draw a vertical line. If a pixel in Ir is skipped, draw a horizontal line. Otherwise, the pixels are matched, and you draw a diagonal line. The plot should end at (1, 1).<br><br>

This was assignment 3 (part 1) in Computer vision course</br></br>
The assignment was done by:

1) Nour el-din hazem
2) Mennatullah Ibrahim
3) Anas Ahmed
