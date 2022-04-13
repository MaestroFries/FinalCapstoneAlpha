# Colour Capture 

This article will aim to summarize our program as means of showcasing what we have learnt over the span of three weeks. In addition, it will serve to compliment the poster if questions are present.

Table of Content:

1. Project Summary
2. Using OpenCV for image analysis & Excel File
3. K-Means Clustering & Elbow Point
4. Colour Capture/Quantization



## Project Summary

The goal of this project is to analyze images using OpenCV, displaying colour data into graphs and a colour palette of prominent colours that will determine optimal results. Functions such as `kmeans()` and `imread()` are used extensively to do such and will be explained in detail. 





## OpenCV for image analysis & Writing to Excel File
Before diving into K-Means Clustering, let's talk about image analysis. In order to analyze image data, we used OpenCV's wide library for computer vision (artifical inteligence). 

```
Mat src1;
 
std::string image_path = samples::findFile("C:/Users/Francis/Documents/images/mario.jpg");

src1 = imread(image_path, IMREAD_COLOR);
```

We can simply access an image by initializing class variable `src1` to the image path, using function `imread()` to read the file.
```
cout << "Select x pixel coordinate:\n";
        cin >> x;
        cout << "X: " << x << "\nSelect y pixel coordinate:\n";
        cin >> y;
        cout << "X: " << x << " | Y: " << y << "\n\n\n\n";

        Vec3b intensity2 = src1.at<Vec3b>(x, y);
        b = intensity2.val[0];
        g = intensity2.val[1];
        r = intensity2.val[2];

```
By using the `.at` function which returns a reference to the specified array element (in this case, 'x' and 'y'), we are able to grab BGR values by calling seperate array indexes. (Vec3b is a vector with 3 byte entries, with each representing each single color channel.)

Now that we are able to access colour data from the image, we can write to an Excel file by using the `fstream` library found in C++.

```
#include <fstream>

...

ofstream outData;

outData.open("CapstoneExceltest.csv", ios::app); // opens the Excel file

outData << intensity2.val[0] << "," << intensity2.val[1] << "," << intensity2.val[2] << endl; //outputs BGR to Excel
```

Learning how to retrieve colour data from an image and to write to an Excel file will serve us well when implimenting the next step: K-Means Clustering.

## K-Means Clustering

As humans, looking at an image and determining which colours are prominent within an image is simple. For machines, doing such a task requires artificial intelligence. Therefore, we decided to use K-Means Clustering as our "go-to" algorithm.

K-Means Clustering is the process of partitioning data points into k clusters in reference to a centroid, or the center point of the cluster (the mean of all data points). Whilst complicated at first, the steps it takes are linear in fashion:

1. Centroids are randomly placed. The number of centroids (K) used is up to the user. Refer to Colour Quantization to determine optimal K value.
2. Each data point is intialized to its closest centroid using euclidean distance.
3. Determine the where the new centroid would be in the cluster by finding the mean.
4. Repeat steps 2-3 until movement has ceased. 


![KMeansClusteringExample](gif.gif)


Because of this, K-Means Clustering can be used to organize BGR data into colour clusters, albeit in a three dimension space (BGR... right?). Luckily, OpenCV has a function dedicated to K-means clustering!

```
double compactness = kmeans(data, k, labels, TermCriteria(TermCriteria::MAX_ITER, 10, 1.0), 3, KMEANS_PP_CENTERS, centers);
```
https://docs.opencv.org/4.x/d1/d5c/tutorial_py_kmeans_opencv.html

`kmeans()` has three output parameters which are of **great importance**: compactness, labels, and centers.

- Compactness refers to the sum of squared distance from each point to their centroid within each cluster. 
- Labels is an array which initalizes each pixel as "0", "1", "2"... depending on which cluster it is inside. 
- Centers is simply the center of clusters. 
- (The other input parameters are used to determine when to stop the algorithm, how many clusters (K), etc...)









Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/MaestroFries/FinalCapstoneAlpha/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
