#include "opencv\cv.h"
#include "opencv\highgui.h"
#include <iostream>
#include <stdio.h>
#include <conio.h>
#include <ctype.h>
#include <direct.h>

using namespace cv;
using namespace std;

IplImage* img;
IplImage* img1;
IplImage* img4;

char orginalname[200];
char filename[200];
char fullname[200];
char dirname[200];
char ext[1];

int a1_right[2] = { 1440,192 };
int a1_left[2] = { 1536,192 };

int a4_right[2] = { 1184,192 };
int a4_left[2] = { 1152,192 };


int width, height;

void A1Divider(IplImage*img) {

	char outname[200];

	cvSetImageROI(img, cvRect(0, 0, a1_left[0], a1_left[1]));
	cvSetImageROI(img1, cvRect(0, 0, a1_left[0], a1_left[1]));

	cvCopy(img, img1, NULL);

	cvResetImageROI(img1);
	cvResetImageROI(img);


	cvSetImageROI(img, cvRect(a1_left[0], 0, a1_right[0], a1_right[1]));
	cvSetImageROI(img1, cvRect(0, 192, a1_right[0], a1_right[1]));

	cvCopy(img, img1, NULL);

	cvResetImageROI(img1);
	cvResetImageROI(img);

	sprintf(outname, "%s//%s", dirname, "a1");
	sprintf(outname, "%s//%s", outname, filename);
	sprintf(outname, "%s%s", outname, ".png");


	cvSaveImage(outname, img1);

}


void A4Divider(IplImage*img) {

	char outname[200];

	cvSetImageROI(img, cvRect(0, 0, a4_left[0], a4_left[1]));
	cvSetImageROI(img4, cvRect(0, 0, a4_left[0], a4_left[1]));

	cvCopy(img, img4, NULL);

	cvResetImageROI(img4);
	cvResetImageROI(img);


	cvSetImageROI(img, cvRect(a4_left[0], 0, a4_right[0], a4_right[1]));
	cvSetImageROI(img4, cvRect(0, a4_right[1], a4_right[0], a4_right[1]));

	cvCopy(img, img4, NULL);

	cvResetImageROI(img4);
	cvResetImageROI(img);

	sprintf(outname, "%s//%s", dirname, "a4");
	sprintf(outname, "%s//%s", outname, filename);
	sprintf(outname, "%s%s", outname, ".png");


	cvSaveImage(outname, img4);

}

void createFolder() {

	char foldername[200];
	sprintf(foldername, "%s//%s", dirname, "a1");
	mkdir(foldername);
	sprintf(foldername, "%s//%s", dirname, "a4");
	mkdir(foldername);
	sprintf(foldername, "%s//%s", dirname, "nord");
	mkdir(foldername);

}

void changePath(char dir[200]) {

	for (int i = 0; dir[i] != '\0'; i++) {
		if (dir[i] != '\\') {
			sprintf(dirname, "%s%c", dirname, dir[i]);
		}
		else {
			strcat(dirname, "//");
		}
	}

}

//C:\Users\t.celik\Desktop\a1

void distributer(IplImage *image) {

	width = img->width;
	height = img->height;

	if (width == 2976 && height == 192) {		// If the resolation is 2976 x 192 handle it as A1
		A1Divider(image);
	}
	if (width == 2336 && height == 192) {		// If the resolation is 2336 x 192 handle it as A4
		A4Divider(image);
	}

	// NORD WILL BE ADDED

}


int main(int argc, char** argv)
{
//	ext[0] = 'y';

	
	img = cvLoadImage("C://Users//t.celik//Desktop//Neuer Ordner//a1//aa_a1.png");
	img1 = cvLoadImage("C://Users//t.celik//Desktop//Neuer Ordner//.png");
	img4 = cvCreateImage(cvSize(1920, 1080), IPL_DEPTH_8U, 3);

	cout << img->height << " " << img->width << " \n";
	cout << img1->height << " " << img1->width << " \n";
	cout << img4->height << " " << img4->width << " \n";
	cvAbsDiff(img, img1, img4);
	cvShowImage("wind4", img4);

	/*

	cout << "Provide the path of the images: ";
	cin.getline(orginalname, 200);
	changePath(orginalname);

	createFolder();

	while (1) {

		cout << "Image name: ";
		cin.getline(filename, 200);

		sprintf(fullname, "%s//%s", dirname, filename);
		strcat(fullname, ".jpg");


		img = cvLoadImage(fullname);

		if (!img) break;


		img1 = cvCreateImage(cvSize(1920, 1080), img->depth, 3);
		img4 = cvCreateImage(cvSize(1920, 1080), IPL_DEPTH_8U, 3);

		distributer(img);
		//	cvShowImage("wind", img);
		//	cvShowImage("wind1", img1);
		//	cvShowImage("wind4", img4);
		//	cvWaitKey(0);

		cvReleaseImage(&img);
		cvReleaseImage(&img1);
		cvReleaseImage(&img4);
		cout << "IMAGE IS CREATED SUCCESSFULLY" << "\n" << "\n";

	}


	*/
	
	cvWaitKey(0);
	
	return 0;
}


