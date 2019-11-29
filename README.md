# Introduction

This is the implementation of **CProSliderCtrl** class, which uses custom draw functionality of CSliderCtrl class to combine Slider Control and Progress bar in one window.

This kind of control is especially required for streaming multimedia applications, because of its less window space requirements and its freezing capability also helps in preventing unbuffered scenes to be viewed/listened.

## Versions

1.0 First release in 2004
1.1 Updated the project to work with recent Visual Studio 2017

## Features

**CSliderCtrl** has the following features:

All the basic features of the standard CSliderCtrl and CProgressCtrl classes
Custom draw borders, thumb, channel and progress bar
Borders could be shown or hidden
The colors could be changed on demand
The slider thumb could be disabled (frozen)
Flicker free progress bar

## Background

The custom draw system heavily uses recursive functions to draw the windows' objects and uses negative exponential random numbers in order to get better colors (will be described later).

A recursive function calls itself for many times, until a control mechanism stops recursion. The prototype of this kind of function is given below:

~~~ c
void Recursion(int val)
{
// computing stuff
val ++;
if(val<=255)Recursion(val);
return;
}
~~~

This is the easiest way to calculate a given color's lighter versions and also gives 3D look to the objects. You may examine the differences by commenting the recursion line inside the function in the given source code.

Since this recursive drawing function lightens the colors every time it calls itself, giving darker color values will result in better colored 3D objects. In order to generate dark colors to use in this recursive function, a negative exponential random number (nexp) generator is implemented. The simulation of the function in Matlab for the input 255 is given in the figure below:

Negative exponential of a given number

How to Use CProSliderCtrl Class
The exported functions are:

~~~ cpp
// Gets the current lower and upper limits, or range, of the progress bar
// control.
void _GetRange(int& nLower, int& nUpper);

// Sets the upper and lower limits of the progress bar control's range and
// redraws the bar to reflect the new ranges.
void _SetRange(short nLower, short nUpper);

// Sets the upper and lower limits of the progress bar control's range and
// redraws the bar to reflect the new ranges.
void _SetRange32(int nLower, int nUpper);

// Sets the background color for the progress bar.
COLORREF _SetBkColor(COLORREF clrNew);

// Sets the thumb color
COLORREF _SetThumbColor(COLORREF clrNew);

// Sets the channel color
COLORREF _SetChColor(COLORREF clrNew);

// Gets the current position of the progress bar.
int _GetPos(void);

// Advances the current position of a progress bar control by a specified
// increment and redraws the bar to reflect the new position.
int _OffsetPos(int nPos);

// Sets the current position for a progress bar control and redraws the
// bar to reflect the new position.
int _SetPos(int nPos);

// Specifies the step increment for a progress bar control.
int _SetStep(int nStep);

// Advances the current position for a progress bar control by the step
// increment and redraws the bar to reflect the new position.
int _StepIt(void);

// De/Freezes the slider and returns the prev. state
BOOL Freeze(void);

// Enables/Disables borders
HRESULT _EnableBorders(BOOL bEnable=TRUE);

// Test if the borders are enabled or not
BOOL _IsEnabled(void); 
~~~

## CProSliderCtrl Class

This class uses the function **OnNMCustomdraw(NMHDR *pNMHDR, LRESULT *pResult)** to handle the custom drawing tasks. When the system sends **TBCD_CHANNEL** message, the class draws the progress bar to the area of the slider channel.

## How to Use CProSliderCtrl Class in Your Applications

Simply add the class to your workspace and define the slider control variable as:

~~~ cpp
// Pro Slider handle
CProSliderCtrl m_ProSlider;
~~~

and use the functions listed above to control progress bar, the functions starting with a "_" are related to progress bar controls.
