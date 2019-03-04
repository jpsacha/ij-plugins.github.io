---
layout: default
title: Connected Threshold Growing Example
nav_order: 2
grand_parent: IJ Plugins Toolkit
parent: 3D Toolkit
permalink: /docs/ijp-toolkit/3D-Toolkit/example-connected-threshold-growing
---

## Connected Threshold Growing Example


The Connected Threshold Growing plugin can be used to perform
    segmentation of 2D and 3D images. The plugin accepts 8 bit and 16 bit gray
    images. To perform segmentation you specify location of seed point (x,y,z),
    and minimum and maximum limits on pixel intensity. Segmented region will
    contain all pixels connected to the seed point which intensity is within
    minimum/maximum intensity limits.</p>

![Grow options dialog]({{ site.url }}/assets/images/ijp-toolkit/3D-Toolkit/grow_options_dialog.jpg)

The example demonstrates segmentation of a 3D MRI image from
    <a href="http://www.bic.mni.mcgill.ca/brainweb/">BrainWeb Project</a>.
    Here is slices 31 of an image used in the example:

![Input image]({{ site.url }}/assets/images/ijp-toolkit/3D-Toolkit/brainweb1_31.jpg)

The image was segmented using only 7 seed points (white matter, gray
    matter, skull, left eye, left lens, right eye, and right lens). White and
    gray matter regions were combined before 3D rendering using ImageJ image
    calculator. 
    (<a href="SmoothRender.java.html">SmoothRender.java</a>)

![Segmented image]({{ site.url }}/assets/images/ijp-toolkit/3D-Toolkit/segmented_brainweb.jpg)

The above 3D rendering was performed using following Java code and VTK library: 

```java
import vtk.*;

import javax.swing.*;
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class SmoothRender extends JPanel {

  static {
    // Load VTK MarchingCubes library, rest is loaded by vtkPanel
    System.loadLibrary("vtkPatentedJava");
  }

  private final String PREFIX = "/data/";

  /**
   * Constructor.
   */
  public SmoothRender() {
    // Setup VTK rendering panel
    vtkPanel renWin = new vtkPanel();

    vtkRenderer ren = renWin.GetRenderer();

    // Create actors for each segmented region

    vtkActor skullActor = createActor(PREFIX + "brainweb1_skull_smooth.vtk", 127, 0.5, 500);
    skullActor.GetProperty().SetOpacity(0.25);
    skullActor.GetProperty().SetColor(1, 1, 1);
    skullActor.GetProperty().SetDiffuse(0.75);
    skullActor.GetProperty().SetSpecular(0.75);
    ren.AddActor(skullActor);

    vtkActor brainActor = createActor(PREFIX + "brainweb1_both_matters.vtk", 127, 0.5, 500);
    brainActor.GetProperty().SetColor(0.9961, 0.75, 0.1);
    brainActor.GetProperty().SetSpecular(1);
    ren.AddActor(brainActor);

    vtkActor rightEyeActor = createActor(PREFIX + "brainweb1_right_eye.vtk", 127, 0.5, 500);
    rightEyeActor.GetProperty().SetColor(1, 1, 1);
    rightEyeActor.GetProperty().SetDiffuse(0.95);
    rightEyeActor.GetProperty().SetSpecular(0.1);
    ren.AddActor(rightEyeActor);

    vtkActor rightRetinaActor = createActor(PREFIX + "brainweb1_right_eye_lens.vtk", 127, 0.10, 100);
    rightRetinaActor.GetProperty().SetColor(0.1, 0.1, 0.1);
    rightRetinaActor.GetProperty().SetSpecular(1);
    ren.AddActor(rightRetinaActor);

    vtkActor leftEyeActor = createActor(PREFIX + "brainweb1_left_eye_smooth.vtk", 127, 0.5, 500);
    leftEyeActor.GetProperty().SetColor(1, 1, 1);
    leftEyeActor.GetProperty().SetDiffuse(0.95);
    leftEyeActor.GetProperty().SetSpecular(0.1);
    ren.AddActor(leftEyeActor);

    vtkActor leftRetinaActor = createActor(PREFIX + "brainweb1_left_eye_lens.vtk", 127, 0.10, 100);
    leftRetinaActor.GetProperty().SetColor(0.1, 0.1, 0.1);
    leftRetinaActor.GetProperty().SetSpecular(1);
    leftRetinaActor.GetProperty().SetSpecularPower(1);
    ren.AddActor(leftRetinaActor);

    // Place renWin in the center of this panel
    setLayout(new BorderLayout());
    add(renWin, BorderLayout.CENTER);
  }

  /**
   * Load region data from a file and create its rendering pipeline.
   */
  private vtkActor createActor(String fileName,
                               int threshold,
                               double targetReduction,
                               int smoothingIterations) {
    vtkStructuredPointsReader reader = new vtkStructuredPointsReader();
    reader.SetFileName(fileName);
    reader.Update();

    vtkMarchingCubes surfaceExtractor = new vtkMarchingCubes();
    surfaceExtractor.SetInput(reader.GetOutput());
    surfaceExtractor.SetValue(0, threshold);
    surfaceExtractor.ComputeNormalsOn();


    vtkDecimatePro deci = new vtkDecimatePro();
    deci.SetInput(surfaceExtractor.GetOutput());
    deci.SetTargetReduction(targetReduction);
    deci.PreserveTopologyOn();

    vtkSmoothPolyDataFilter smoother = new vtkSmoothPolyDataFilter();
    smoother.SetInput(deci.GetOutput());
    smoother.SetNumberOfIterations(smoothingIterations);

    vtkPolyDataNormals normals = new vtkPolyDataNormals();
    normals.SetInput(smoother.GetOutput());
    normals.FlipNormalsOn();

    vtkPolyDataMapper mapper = new vtkPolyDataMapper();
    mapper.SetInput(normals.GetOutput());
    mapper.ScalarVisibilityOff();

    vtkActor actor = new vtkActor();
    actor.SetMapper(mapper);

    return actor;
  }

  /**
   * 
   */
  public static void main(String[] args) {
    SmoothRender panel = new SmoothRender();

    JFrame frame = new JFrame("SmoothRender");
    frame.addWindowListener(new WindowAdapter() {
      public void windowClosing(WindowEvent e) {
        System.exit(0);
      }
    });
    frame.getContentPane().add("Center", panel);
    frame.pack();
    frame.setVisible(true);
  }
}
```


