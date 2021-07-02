#include "headers.h"
#include <math.h>
#include <stdio.h>

// Convert image to grayscale
void grayscale(int height, int width, RGBTRIPLE image[height][width])
{
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            // Just finding average of RBG and assigning it to all three values.
            float avg = (image[i][j].rgbtBlue + image[i][j].rgbtGreen + image[i][j].rgbtRed) / 3.0;
            avg = round(avg);
            image[i][j].rgbtBlue = avg;
            image[i][j].rgbtGreen = avg;
            image[i][j].rgbtRed = avg;
        }
    }
    return;
}

// Reflect image horizontally
void reflect(int height, int width, RGBTRIPLE image[height][width])
{
    for (int i = 0; i < height; i++)
    {
        // Its just reversing the array in every row.
        for (int j = 0; j < width / 2; j++)
        {
            int temp1 = image[i][j].rgbtRed;
            int temp2 = image[i][j].rgbtGreen;
            int temp3 = image[i][j].rgbtBlue;

            image[i][j].rgbtRed = image[i][width - 1 - j].rgbtRed;
            image[i][j].rgbtGreen = image[i][width - 1 - j].rgbtGreen;
            image[i][j].rgbtBlue = image[i][width - 1 - j].rgbtBlue;

            image[i][width - 1 - j].rgbtRed = temp1;
            image[i][width - 1 - j].rgbtGreen = temp2;
            image[i][width - 1 - j].rgbtBlue = temp3;
        }
    }
    return;
}

// Blur image
void blur(int height, int width, RGBTRIPLE image[height][width])
{
    // We have to first copy the values of image array in a temp struct because once hese values get manipulted we cant get original values again.
    typedef struct
    {
        int red;
        int green;
        int blue;
    }
    Temp;

    Temp temp[height][width];

    // copying original values to temp
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            temp[i][j].red = image[i][j].rgbtRed;
            temp[i][j].green = image[i][j].rgbtGreen;
            temp[i][j].blue = image[i][j].rgbtBlue;
        }
    }

    // using blur box algorithm to update values
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int startr, endr;
            int startc, endc;

            // finding rows and cloumns for every pixel according to the requirement.

            startr = (i - 1) > -1 ? (i - 1) : i;
            endr = (i + 1) < height ? (i + 1) : i;

            startc = (j - 1) > -1 ? (j - 1) : j;
            endc = (j + 1) < width ? (j + 1) : j;
            float sum1 = 0;
            float sum2 = 0;
            float sum3 = 0;
            float ctr = 0;

            for (int k = startr; k <= endr; k++)
            {
                for (int l = startc; l <= endc; l++)
                {
                    sum1 += temp[k][l].red;
                    sum2 += temp[k][l].green;
                    sum3 += temp[k][l].blue;
                    ctr++;
                }
            }

            image[i][j].rgbtRed = round(sum1 / ctr);
            image[i][j].rgbtGreen = round(sum2 / ctr);
            image[i][j].rgbtBlue = round(sum3 / ctr);

        }
    }
    return;
}

// Detect edges
void edges(int height, int width, RGBTRIPLE image[height][width])
{
    // We have to first copy the values of image array in a temp struct because once these values get manipulted we cant get original values again.
    typedef struct
    {
        int red;
        int green;
        int blue;
    }
    Temp;

    Temp temp[height][width];

    // copying original values to temp
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            temp[i][j].red = image[i][j].rgbtRed;
            temp[i][j].green = image[i][j].rgbtGreen;
            temp[i][j].blue = image[i][j].rgbtBlue;
        }
    }

    // Now using the image convolution method i.e using kernels and updating the actual image struct pixels.
    // Storing kernels GX and GY.
    int kr1[3][3] = {{-1, 0, 1}, {-2, 0, 2}, {-1, 0, 1}};
    int kr2[3][3] = {{-1, -2, -1}, {0, 0, 0}, {1, 2, 1}};

    // Updating every pixel using kernels.
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            int startr, endr;
            int startc, endc;

            // finding rows and cloumns for every pixel according to the requirement.

            startr = (i - 1);
            endr = (i + 1);

            startc = (j - 1);
            endc = (j + 1);


            // six total values . gx for red,gree and blue and similary for gy another three.
            // Gx
            double kr1_red_sum = 0;
            double kr1_green_sum = 0;
            double kr1_blue_sum = 0;

            //Gy
            double kr2_red_sum = 0;
            double kr2_green_sum = 0;
            double kr2_blue_sum = 0;

            for (int k = startr, a = 0; k <= endr; k++, a++)
            {
                for (int l = startc, b = 0; l <= endc; l++, b++)
                {
                    if (!(k < 0 || k >= height || l < 0 || l >= width))
                    {
                        // Gx values
                        kr1_red_sum = kr1_red_sum + (temp[k][l].red * kr1[a][b]);
                        kr1_green_sum = kr1_green_sum + (temp[k][l].green * kr1[a][b]);
                        kr1_blue_sum = kr1_blue_sum + (temp[k][l].blue * kr1[a][b]);

                        // Gy Values
                        kr2_red_sum = kr2_red_sum + (temp[k][l].red * kr2[a][b]);
                        kr2_green_sum = kr2_green_sum + (temp[k][l].green * kr2[a][b]);
                        kr2_blue_sum = kr2_blue_sum + (temp[k][l].blue * kr2[a][b]);
                    }

                }
            }

            // Final G = sqrt(gx2 + gy2)
            double g_red = sqrt((kr1_red_sum * kr1_red_sum) + (kr2_red_sum * kr2_red_sum));
            double g_green = sqrt((kr1_green_sum * kr1_green_sum) + (kr2_green_sum * kr2_green_sum));
            double g_blue = sqrt((kr1_blue_sum * kr1_blue_sum) + (kr2_blue_sum * kr2_blue_sum));

            if (g_red > 255)
            {
                g_red = 255;
            }
            else
            {
                g_red = round(g_red);
            }

            if (g_green > 255)
            {
                g_green = 255;
            }
            else
            {
                g_green = round(g_green);
            }

            if (g_blue > 255)
            {
                g_blue = 255;
            }
            else
            {
                g_blue = round(g_blue);
            }

            // Finally Updating the image with updated Gx and Gy channels for red, green and blue.
            image[i][j].rgbtRed = g_red;
            image[i][j].rgbtGreen = g_green;
            image[i][j].rgbtBlue = g_blue;

        }
    }
    return;
}
