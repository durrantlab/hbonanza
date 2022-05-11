# Introduction

We here present a new computer program called HBonanza (hydrogen-bond analyzer)
that aids the analysis and visualization of hydrogen-bond networks. HBonanza,
which can be used to analyze single structures or the many structures of a
molecular dynamics simulation, is open source and python implemented, making it
easily editable, customizable, and platform independent. Additionally, the
algorithm is fast and easy to use. Unlike many other freely available
hydrogen-bond analysis tools, HBonanza provides not only a text-based table
describing the hydrogen-bond network, but also a Tcl script file to facilitate
visualization in [VMD](http://www.ks.uiuc.edu/Research/vmd/), a popular
molecular visualization program. Visualization in other programs is also
possible.

## Download

HBonanza is a Python script, and so requires a [Python
interpreter](http://www.python.org/getit/). The program has been tested on
Linux, Mac OSX, and Windows. [Download a copy of HBonanza](HBonanza_1.01.py)
here.

## Tutorial

### Introduction

HBonanza is a computer program that can be used to perform hydrogen-bond
analyses of molecular dynamics trajectories. It accepts a PDB file of the
trajectory as input, and outputs a TCL visualization script that can be loaded
into VMD using either the Tk Console or the -e option from the command line.
Trajectory files in other formats can be converted to the PDB format easily
using VMD's "Save Coordinates..." option. The generated TCL file also contains
comments to aid the user if he or she wishes to use other molecular
visualization software.

### Command-line Parameters

A number of command-line options are available:

<table>
   <tr>
      <td width="35%">
         <tt>-help</tt>
      </td>
      <td>
         Displays this help documentation.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-trajectory_filename</tt>
      </td>
      <td>
         The filename containing the trajectory, in PDB format.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-output_basename</tt>
      </td>
      <td>
         The initial string used to begin the names of all output files.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-single_frame_filename</tt>
      </td>
      <td>
         The filename of a PDB containing a single frame, onto which hydrogen bond visualization will be mapped.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-number_processors</tt>
      </td>
      <td>
         The number of processors to use when performing the hydrogen-bond analysis of the trajectory. By default, all processors on the system are used.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-hydrogen_bond_frequency_cutoff</tt>
      </td>
      <td>
         In creating visualization files, discard all hydrogen bonds that are formed less frequently than this value.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-hydrogen_bond_distance_cutoff</tt>
      </td>
      <td>
         Consider only hydrogen bonds whose donor-acceptor distances are less than or equal to this value.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-hydrogen_bond_angle_cutoff</tt>
      </td>
      <td>
         Consider only hydrogen bonds whose hydrogen-donor-acceptor angles are less than or equal to this value.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-just_immediate_connections</tt>
      </td>
      <td>
         Rather than branching out recursively to identify the entire hydrogen-bond network that supports the seed residues, consider only those residues that are immediately connected to the seeds via hydrogen bonds.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-seed_residues or -seed_residue</tt>
      </td>
      <td>
         Consider only hydrogen bonds connected to residues with these IDs, or connecting residues that connect to these residues through hydrogen bonds. Multiple seed residues can be specified by evoking this tag multiple times (see example below). If no seed residues are specified, the program will visualize all hydrogen bonds, regardless of connectivity.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-write_column</tt>
      </td>
      <td>
         The program writes a PDB file containing the hydrogen-bond frequencies in either the occupancy or beta columns. If this tag is set to "O", the frequencies are written to the occupancy column. If set to "B", they are written to the beta column.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-low_frequency_color_r</tt>
      </td>
      <td>
         Hydrogen bonds are colored according to frequency. Colors are specified using RGB (red, green, blue) values. This tag specifies the R value of the color associated with the lowest frequency displayed (given by -hydrogen_bond_frequency_cutoff). Valid values range from 0 to 255.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-low_frequency_color_g</tt>
      </td>
      <td>
         The G value of the color associated with the lowest frequency displayed. Valid values range from 0 to 255.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-low_frequency_color_b</tt>
      </td>
      <td>
         The B value of the color associated with the lowest frequency displayed. Valid values range from 0 to 255.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-high_frequency_color_r</tt>
      </td>
      <td>
         The R value of the color associated with 100% persistent hydrogen bonds. Valid values range from 0 to 255.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-high_frequency_color_g</tt>
      </td>
      <td>
         The G value of the color associated with 100% persistent hydrogen bonds. Valid values range from 0 to 255.
      </td>
   </tr>
   <tr>
      <td>
         <tt>-high_frequency_color_b</tt>
      </td>
      <td>
         The B value of the color associated with 100% persistent hydrogen bonds. Valid values range from 0 to 255. 
      </td>
   </tr>
</table>

### Description of Output Files

Assuming the command-line parameter `-output_basename` is set to "output.", the
following files will be created:

<table>
   <tr>
      <td>
         <tt>output.average_hbonds</tt>
      </td>
      <td>
         A two-column text file describing all hydrogen bonds in the trajectory, regardless of the -hydrogen_bond_frequency_cutoff tag. The first column is a triplet indicating the atom indices of the atoms that form each hydrogen bond, and the second column is the frequency with which that particular hydrogen bonds occurs across all the frames of the trajectory.
      </td>
   </tr>
   <tr>
      <td>
         <tt>output.frame_by_frame_hbonds.csv</tt>
      </td>
      <td>
         A comma-separated-values file (CSV file) indicating which hydrogen bonds are present in each of the frames of the trajectory. CSV files can be loaded by a number of popular spreadsheet programs, including Microsoft Excel and Open Office Calc.
      </td>
   </tr>
   <tr>
      <td>
         <tt>output.hbond_averages_in_occupancy_column.pdb<br/>
         OR<br/>
         output.hbond_averages_in_beta_column.pdb</tt>
      </td>
      <td>
         A pdb file identical to the file specified by the -single_frame_filename tag, except that the average frequencies with which relevant hydrogen-bond atoms appear in hydrogen bonds across the trajectory are written in the occupancy or beta column, depending on the value given by the -write_column tag. This file may facilitate visualization in programs other than VMD, if desired.
      </td>
   </tr>
</table>

### Examples of Usage

<ins>Example 1</ins>: Load in a trajectory named "traj.pdb". Define hydrogen bonds
to be those that 1) have donor-acceptor distances less than or equal to 3.0
angstroms, 2) have hydrogen-donor-acceptor angles less than or equal to 30
degrees, and 3) are formed in at least 50% of the trajectory frames. Show the
visualization based on the coordinates in the single-frame PDB file
"first_frame.pdb". Write output files using filenames that start with "output."
One of these output filenames will be a PDB file with the hydrogen-bond
frequencies listed in the occupancy column. Because no seed residues are
specified, all hydrogen bonds in the trajectory will be visualized. Hydrogen
bonds that appear in 50% of the trajectory frames will appear white, and those
that appear in 100% of the trajectories frames will be green. The program output
will be directed to the file "visualize.tcl" (in UNIX-based systems), which can
then be visualized in VMD.

```bash
python HBonanza.py -trajectory_filename traj.pdb -hydrogen_bond_distance_cutoff 3.0 -hydrogen_bond_angle_cutoff 30 -hydrogen_bond_frequency_cutoff 0.5 -single_frame_filename first_frame.pdb -output_basename output. -write_column O -low_frequency_color_r 255 -low_frequency_color_g 255 -low_frequency_color_b 255 -high_frequency_color_r 0 -high_frequency_color_g 255 -high_frequency_color_b 0 > visualize.tcl

/path/to/VMD/executable/VMD -e visualize.tcl
```

<ins>Example 2</ins>: Same as above, except only hydrogen bonds connected to
residues with IDs 156 and 234, or connecting residues that connect to these
residues through hydrogen bonds, will be visualized.

```bash
python HBonanza.py -trajectory_filename traj.pdb -hydrogen_bond_distance_cutoff 3.0 -hydrogen_bond_angle_cutoff 30 -hydrogen_bond_frequency_cutoff 0.5 -single_frame_filename first_frame.pdb -output_basename output. -write_column O -low_frequency_color_r 255 -low_frequency_color_g 255 -low_frequency_color_b 255 -high_frequency_color_r 0 -high_frequency_color_g 255 -high_frequency_color_b 0 -seed_residue 156 -seed_residues 234 > visualize.tcl
```

<ins>Example 3</ins>: If the -single_frame tag is not specified, the program will
automatically use the first frame from the trajectory specified by the
-trajectory_filename tag.

```bash
python HBonanza.py -trajectory_filename traj.pdb -hydrogen_bond_distance_cutoff 3.0 -hydrogen_bond_angle_cutoff 30 -hydrogen_bond_frequency_cutoff 0.5 -output_basename output. -write_column O -low_frequency_color_r 255 -low_frequency_color_g 255 -low_frequency_color_b 255 -high_frequency_color_r 0 -high_frequency_color_g 255 -high_frequency_color_b 0 -seed_residue 156 -seed_residues 234 > visualize.tcl
```


<ins>Example 4</ins>: If an analysis has been performed previously, it's not
necessary to specify the -single_frame and -trajectory_filename tags, as long as
the same -output_basename and -write_column tags are used. The trajectory
hydrogen-bond information will be read from the previously created file
"{output_basename}.average_hbonds", and the -single_frame tag will be
automatically set to the previously created
"{output_basename}.hbond_averages_in_{write_column}*_column.pdb" file.

```bash
python HBonanza.py -hydrogen_bond_distance_cutoff 3.0 -hydrogen_bond_angle_cutoff 30 -hydrogen_bond_frequency_cutoff 0.5 -output_basename output. -write_column O -low_frequency_color_r 255 -low_frequency_color_g 255 -low_frequency_color_b 255 -high_frequency_color_r 0 -high_frequency_color_g 255 -high_frequency_color_b 0 -seed_residue 156 -seed_residues 234 > visualize.tcl
```

<!-- ## Sample Visualization

With only minor modifications, the default HBonanza output visualization file
can be rendered as below. The ligand is shown in thick licorice, protein
residues thought to participate in hydrogen-bond networks involving the ligand
are shown in thin licorice, and the entire protein is shown in cartoon, colored
according to secondary structure. Hydrogen bonds are colored in shades of green
according to the frequency with which they appear in the many frames of the MD
trajectory. The coloring ranges from white, those bonds present in 75% of the MD
frames, to green, those bonds present in 100% of the frames. -->

<!-- <center><img src="images/sample_visualization.jpg" alt="" class="aligncenter border" /></center> -->
