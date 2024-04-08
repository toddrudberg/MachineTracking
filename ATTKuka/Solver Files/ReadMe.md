# ATT_kuka_nominal Configuration

This document provides an overview of the `ATT_kuka_nominal.config.xml` file.

## Overview

The `ATT_kuka_nominal.config.xml` file is an XML configuration file which describes the kinematics of a KUKA KR60 robot. 


## Structure

The XML file is structured as follows:

There are 2 bigger sections, Variables and Transforms.  The variables section contains all the data that you can use later in the Transforms section.  

There are a number of keywords and protected vairables (I have forgotten many of them, but I will describe what I remember)
      <_Debug value="False" /> - if set to true, I think it disiplays some debug information if your file is jacked up.

      <lJ6z value="0.0" note="oriented to +z axis"/> - note is a keyword that allows you to describe what this variable represents
      <TP10Z value="147.41" Hide="true" /> - Hide - actually should be called "ReadOnly"  The solver can see this and won't attempt to optimize the output using it.  THis is good for metrology points that are valued by an outside device and are have a fixed value. 

  <variables>


## 📌 **Static Variable Section**
    <static multiply="10000.0">
      <_Debug value="False" />
    
      <baseBlocks value="76.2" />  
      <KRH60zoff value="815" />

        ## 📌 **Description**
    `value:` the current value for this vairable.  The variable name is used in transforms below.
    `Hide:` if set to true, hides this variable from the solver.  treat this as a read only variable.


    </static>

## 📌 **Machine Axes Section**

        <machineaxes multiply="1000000.0"> - ignore the multiply for now.
        <J1m rotary="True" scalefactor="0.000000" offset="0.000000" />
        
        ## 📌 **Description**
            `J1m:`  The string name of the axis.  This is used as an argument in the transforms below.
            `rotary:` true or false.  Indicates if the axis is revolute or linear. 
            `offset:` I no longer use offset, this was the machines homing offset.  I now code it in the vairables above and for all rotary axes would be the jxmrzoff.  I always align the rZ value with the axis' rotation
            `scalefactor:` this is the scaling factor for the machine.  In the Machine class it multiplies the machine axis by 1+scalefactor
        </machineaxes>
  </variables>

## 📌 **Transforms Section**
    <sixdof n="2" X="0" Y="0" Z="KRH60zoff" rX="j1mrxoff" rY="j1mryoff" rZ="j1mrzoff-J1m" />
        
        
        ## 📌 **Description**
        Transforms reads the varibles defined above.  You can write most equations you can think of.  sin, cos, tan.  
        
        `"="` is a logical operator.  As you can see in the full file, I have defined `TP` as an axis.  You can use this to chose which metrology location is the one the measurement was taken from.  

        transform arguments: X, Y, Z, rX, rY, rZ.  This builds a 4x4 linear homogeneous transform.  It is first translate then rotate.  The rX, rY and rZ arguments are Euler arguments and go in the order of operation of rX, rY, & rZ.  




**Here's the robot's full file.  Note that there are no "Comp Tables".  This is a subject for another machine or if we add a linear axis - or perhaps a rotary table.**

<?xml version="1.0" encoding="utf-8"?>
<EITransform>
  <variables>
    <static multiply="10000.0">
      <_Debug value="False" />
    
      <baseBlocks value="76.2" />  
      <KRH60zoff value="815" />
      
      <j1mrxoff value="0.00" />
      <j1mryoff value="0.0"  />
      <j1mrzoff value="0.0" note="oriented to +y axis" />
      <lJ1x value="350" note="+xoffset" />
      <lJ1y value="0" note="" />
      <lJ1z value="0" note="" />

      <j2mrxoff value="0.00" />
      <j2mryoff value="0.0" />
      <j2mrzoff value="0.0" note="oriented to +x axis" />
      <lJ2x value="850" note="xoffset" />
      <lJ2y value="0.0" note="" />
      <lJ2z value="0.0" />

      <j3mrxoff value="0.00" />
      <j3mryoff value="0.0" />
      <j3mrzoff value="0.0" note="oriented to +x axis" />
      <lJ3x value="820" note="+xoffset" />
      <lJ3y value="-145" note="" />
      <lJ3z value="0.0" />

      <j4mrxoff value="0.00" />
      <j4mryoff value="0.0" />
      <j4mrzoff value="0.0" />
      <lJ4x value="0.0" note="" />
      <lJ4y value="0.0" note="" />
      <lJ4z value="0.0" note="" />

      <j5mrxoff value="0.00" />
      <j5mryoff value="0.0" />
      <j5mrzoff value="0.0" />
      <lJ5x value="170" note="+xoffset" />
      <lJ5y value="0.0" note="" />
      <lJ5z value="0.0" note="" />

      <j6mrxoff value="0.00" />
      <j6mryoff value="0.0" />
      <j6mrzoff value="0.0" />
      <lJ6x value="0.0" note="+xoffset" />
      <lJ6y value="0.0" note="+yoffset" />
      <lJ6z value="0.0" note="oriented to +z axis"/>

      <BaseShift X="0" Y="0" Z="0" rX="0.0" rY="0.0" rZ="0.0" note="EI machines are roughly aliged with the cell system, so we move the machine's base."/>
      <PartXform X="0" Y="0" Z="0" rX="0.0" rY="0.0" rZ="0.0" note="some cells have a transform back to the machine space, so we adjust this and fix the machine's base" />

      <TP10X value="-42.46" Hide="true" />
      <TP10Y value=".24" Hide="true" />
      <TP10Z value="147.41" Hide="true" />
    </static>
    <machineaxes multiply="1000000.0">
      <J1m rotary="True" scalefactor="0.000000" offset="0.000000" />
      <J2m rotary="True" scalefactor="0.00000" offset="0.000000" />
      <J3m rotary="True" scalefactor="0.0000" offset="0.000000" />
      <J4m rotary="True" scalefactor="0.000000" offset="0.000000" />
      <J5m rotary="True" scalefactor="0.0000" offset="0.000000" />
      <J6m rotary="True" scalefactor="0.0" offset="0.000000" />
      <TP rotary="False" scalefactor="0.000000" offset="0.000000" Hide="true" />
    </machineaxes>
  </variables>
  <TransForms>
    <sixdof n="2" X="0" Y="0" Z="KRH60zoff" rX="j1mrxoff" rY="j1mryoff" rZ="j1mrzoff-J1m" />
    <sixdof n="3" X="lJ1x" Y="lJ1y" Z="lJ1z" rX="-90+j2mrxoff" rY="j2mryoff" rZ="j2mrzoff+J2m" />
    <sixdof n="4" X="lJ2x" Y="lJ2y" Z="lJ2z" rX="j3mrxoff" rY="j3mryoff" rZ="j3mrzoff+J3m" />
    <sixdof n="5" X="lJ3x" Y="lJ3y" Z="lJ3z" rX="j4mrxoff" rY="90+j4mryoff" rZ="j4mrzoff-J4m" />
    <sixdof n="6" X="lJ4x" Y="lJ4y" Z="lJ4z" rX="j5mrxoff" rY="-90+j5mryoff" rZ="j5mrzoff+J5m" />
    <sixdof n="7" X="lJ5x" Y="lJ5y" Z="lJ5z" rX="j6mrxoff" rY="90+j6mryoff" rZ="j6mrzoff-J6m" />
    <sixdof n="8" X="0" Y="0" Z="0" rX="0" rY="0" rZ="90" />    
    <sixdof n="9" X="TP10X*(TP=10)" Y="TP10Y*(TP=10)" Z="TP10Z*(TP=10)" rX="0" rY="0" rZ="0" />
  </TransForms>
</EITransform>

