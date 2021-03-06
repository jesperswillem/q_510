#Qprep version history

## Release version 5.04

- Added command to define simulation and solvation centre by selecting
  mass weighted centre of a selection of atoms or residues, option
  "mass" in the "boundary" and "solvate" commands.

- Added command to solvate sphere with  the same centre as set for the
  boudary, option "boundary" in the "solvate" command.

## Beta version 5.03

- Added 'average' command. Calulates an average structure from a
  trajectory file, and writes a pdb-file.

- Added info about total charge of the system in 'maketop' command.
     
## Beta version 5.01

- Moved definition of solvent restrain shell from topology to Qdyn input file.
  The shell can thus be changed without remaking the topology.
  It is defined under [sphere], keyword: shell_radius.
  Shell is calculated from topology coordinates unless overridden with
  [file] restraint. Furthermore the default is changed to NO shell.
  See manual for details.
  
- Added command 'boundary'. Defines the boundary of the molecular system.
  See manual for details.  

- Additions made to 'solvate' command. Qprep can solvate either spherical
  or periodic boundary. The boundary is defined with the 'boundary' command.
       
- Qprep does not print excluded solute charge groups in the topology file. 
  It has no effect to the user.

- Conjugated gradient algorithm implemented for hydrogen positioning. 
       
## Release 4.22, 00-08-25
- Will now silently accept library/parameter files not ending in newline.

## Beta version 4.21, 00-08-24
- Corrected problem with counting atoms in PDB files.
- Comment lines (starting with one of !#*) are now accepted but ignored in Qprep input.

## Beta version 4.20, 00-08-21
    Some syntax changes to compile with the Portland Group Linux F90 
    compiler.


## Beta version 4.19, 00-07-17
    Library entries used as solvent are now identified directly in the 
    library, as in the example below
    [info]
        solvent     1
        density     0.0335
    The preference value rho_solvent has been removed.
    A preference value for the average solute heavy atom number density,
    solute_density has been added.
    The solvate file command can now be used with e.g. a file with
    methanol coordinates (but still only 3-atom solvents.)
    
    maketop without a prior call to solvate does no longer produce
    an error, just a warning that no solvent has been added.

## Beta version 4.18, 00-07-10
    Changed exclusion of solvent molecules to beyond simulation sphere radius
    +2�, not (the at this stage sometimes undefined) solvent radius.
    Changed classification of solvent types from SPC, TIP3P, general to
    SPC, 3-atom, general.
    Removed atom type codes for water O and H from topology.
    Added flag for use of switching atoms to topology. Switching atom 
    designations will no longer be set to zero for force fields not using
    them for cut-off. For solvent molecules, radial restraints are applied to 
    the switching atom. For water, this means O as before.

## Beta version 4.17, 00-06-13
    Corrected problem with counting explicitly defined impropers in the solute.

## Beta version 4.15, 00-03-02
    Added count of solute torsions and impropers to topology.

## Beta version 4.13, 00-02-22
    Corrected problem with 'mask restrained'.

## Beta version 4.12, 00-01-13
    Added atom mask used when writing PDB files. By default, the mask includes
    all atoms. The new command mask is used to add atoms to the mask or to 
    clear the mask. The syntax of the mask command is similar to that for
    trajectory mask definition in Qdyn4 input files:
    mask [properties] [residue] [first [last]]
      properties are one or more of:
        solute: only solute atoms
        excluded: atoms outside the simulation sphere
        restrained: atoms outside the simulation sphere and in the restrained shell
        heavy: heavy (non-hydrogen) atoms
      Each property may be negated by prepending not.
      If no property is given, all atoms are selected.
      residue: first and last are residue numbers, not atom numbers
      first: first atom/residue of a sequence.
      last: last atom/residue of a sequence.
      If first and last are omitted, first=1 and last=number of atoms/residues
      If last is omitted, last=first.
    Use 'mask none' to clear the mask.
    (Note that the property keyword all has been removed - it is the default.)
    Added capability to read DCD trajectories. The new command trajectory
    is used to open a DCD trajecory file.
    Added user-settable preferences for water density and random seeds.

## Beta version 4.11, 00-01-12
    Changed pointer dissociation in prmfile.f90 due to problems with LINUX/
    NAG f95.

## Beta version 4.09, 00-01-11
    Corrected problem reading old parameter files.

## Beta version 4.08, 00-01-09
    GAP lines are no longer required between molecules where either the last
    residue of the first molecule has no tail atom designation or the first 
    residue of the new molecule has no head atom.
    GAP are thus required only to explicitly break a chain, not between 
    solvent molecules etc.
    This emphasizes the importance of the molecule list generated while 
    reading the PDB file - check it to see that the correct molecule
    boundaries are generated!
    Also corrected problem with truncation of the tolology title.
    The atoms numbers in the addbond command may now be entered in the
    residue_number:atom_name format e.g.
    addbond 83:SG 141:SG

## Beta version 4.07, 00-01-08
    Corrected problem with repeated prompting for solvent sphere data.

## Beta version 4.06, 00-01-07
    Corrected problem with hydrogens being regenerated although present in the
    PDB file. Changed the xlink routine: it should now be called BEFORE 
    calling maketop.
    
## Beta version 4.05, 00-01-06
    Corrected problem with extra bonds (addbond/xlink) which were not placed 
    correctly in the bond list.
    Changed the addbond command: the new usage syntax is
    addbond atom1 atom2
    addbond atom3 atom4
    etc. Also added a command to clear extra bonds: clearbond.
    Addbond now calculates the bond distance and warns if it exceeds the (new)
    max_xlink preference value (default 2.1 �).
    If maketop is called with no prior solvation, the program prompts for
    solvent radius and centre. 
    
## Beta version 4.04, 99-12-19
    Overloading of library entries: The a new definition of an entry 
    (recurrence of its name) leads to removal of the first entry.

## Beta version 4.03, 99-12-17
    Added user-settable variables controlling solvent grid spacing and solvent
    to solute packing distance. Use the command prefs to show and set to
    change.
    When countpdb fails, the PDB file is closed so that is can be edited
    and saved. 
    Maketop with no force field loaded now gives an error message.

## Internal version 4.00, 99-12-13
    When solvating from restart file, normal 'packing' limitation against 
    solute heavy atoms is disabled. 

## Internal version 3.99, 99-11-20
    Added solvent type information (SPC/TIP3p/general) and water atom types
    to topology. The general solvent type is not implemented yet.
    When loading a topology all libraries loaded when it was created will be
    auto-loaded, unless other libraries are loaded.

## Internal version 3.98, 99-11-05
    Completely rewrote code to generate hydrogens. The new algorithm does
    a steepest descent energy minimisation of bond and angle terms involving
    the H with respect to the H position. A torsion angle may be specified
    in a new section [build_rules] for each residue in the library, to
    control the geometry of hydrogens. This is used e.g to make ASN and GLN 
    amide groups planar and to get the correct geometry of carboxylic acid
    groups. Example: 
    {AR+}               !Residue name
    ...
    ...
    [build_rules]
        torsion HH11 NH1 CZ NH2 0.
        torsion HH21 NH2 CZ NH1 0.
    Here, the two torsions with a single energy minimum at 0 degrees should be
    taken into account in the energy minisation.

## Internal version 3.97, 99-10-14
    Added code to exclude atoms and set up a restrained shell. This 
    information is written to the topology file.
    The maketop routine will prompt for simulation sphere radii & centre.
    
## Internal version 3.96, 99-10-12
    Corrected problem with tryptophan hydrogen generation.
    Added code to solvate from a restart file. Only solvent coordinates
    are read from the restart file.
    The readx command now works only if the number of atoms in the topology
    and restart files are equal.

## Internal version 3.95, 99-10-11
    Corrected bug in sub ortvec, now generates random vectors in any direction
    Cleaned up randm function to store seed internally and allow re-seeding.
    Added code to keep track of solute and solvent bond and angle count.
    Added solvation algorithms for solvation using grid and from file
    (like previously in Qdyn)
    Solvent molecules will now be explicitly included in the topology. 
    Solvent hydrogens will be generated using a separate pseudo-random number 
    sequence to allow reproducible solvent generation independent of 
    the number of solute hydrogens generated.
    The solvent centre can be specifile using x y z or 
    residue_number:atom_name.
    Removed special water atom/bond/angle type variables from topology.
    Modified the adding of hydrogens to atoms with two connections (water O:s)
    so that both bond length and angle are taken from the force field.
    Corrected the generation of long-range 1-4 neighbour list so no 
    duplicate entries are created.
    Clarified the logic of the angle-generation code (no change of function).
    An enpty PDB file can now be read without crashing Qprep.
    In PDB files written, the selected solvent residue name and atom names
    will be used.

## Release 3.73, 99-10-01
    Added generation of CONECT records in PDB files for fragments designated
    PDBtype HETATM. Example:
    {MTX}
        [info]
            SYBYLtype  GROUP
            PDBtype    HETATM
        [atoms]         !  methotrexate

## Release 3.70, 99-08-03
    Added automatic generation of cross-linking bonds (S-S bridges) by means
    of a new command xlink.

## Release 3.69, 99-05-17
    Fixed problem with undefined residue codes in listres and problem with 
    writepdb.

## Release 3.68, 99-05-12
    Bond, head/tail and charge group definitions in the library may now be 
    done by means of atom names instead of atom numbers. (Atom numbers are
    still OK and names & numbers may be mixed.) Example:
    {GLY}               !Residue name
        [atoms]
             1 N      N.pep       -0.280 !At. no., name, type, charge
             2 H      H            0.280
             3 CA     C.3H2        0.000
             4 C      C.2          0.383
             5 O      O.2         -0.383
        [bonds]         !bonds (intra-residue)
           N    H                  !connect i -- j
           N    CA 
           CA   C  
           C    O  
        [connections]   !how to bond to previous and next
            head     N  
            tail     C  
        [charge_groups] !charge groups, with switch atom first
            N    H                    !Atom list
            CA 
            C    O  


## Release 3.67, 99-05-12
    Fixed problem with writeprb routine writing two GAP lines after solute.
    Added checks to avoid problems with multiple GAP lines.

## Release 3.66, 99-05-10
    Fixed problem with topology bormat (bonds) when #atoms >= 10000.
    Fixed problem reading old-style parameter file related to Urey-Bradley
    angle parameters.

## Release 3.65, 99-04-21
    Fixed allocation problem when calling readtop repeatedly.

## Release 3.63, 99-03-15
    Can now handle up to 1 improper for each atom.
    Water bond code, angle code and charge are now written to the topology.

## Release 3.61, 99-03-02
    Fixed problem with GAP between last solute residue and first water.
    Fixed memory problem when reading topology and then adding bonds.
    Fixed memory problems when reading  and re-making topology.

## Release 3.60, 99-02-24
    New code for looking up improper parameters to handle different force 
    fields properly.

## Release 3.59, 99-02-23
    Fixed problem caused by bug in the code for reading the coulumb_constant.
    Fixed setting of default values when reading old parameter file.
    Fixed output format error in PDB check routine.

## Release 3.58, 99-02-18
    Fixed memory problem causing program crash when re-reading PDB file.
    Fixed problem with reading old parameter files

## Release 3.57, 99-02-12
    Removed the limitation on the number of atoms, residues and molecues.
    Qprep can now - even without recompilation - handle any number of
    atoms.
    New command cleartop to make it possible to switch to another parameter
    file. The command just makes the Qprep forget the name of the param.
    file.

## Release 3.56, 99-02-12
    Changed the maximal number of molecules and residued in Qprep to the same
    as the max. number of atoms, currently 10000.

## Release 3.53, 99-02-03
    Corrected improper parameter lookup function to ensure correct handling of
    wild-cards. 

## Interim version 3.52, 99-01-28
    Improper parameters are defined using 4 atom types instead of 2, to conform 
    with Amber95 and Charmm22. The atoms are listed in the same order as for torsions.
    A torsion (proper or improper) is the angle between the plane defined by
    atoms i-j-k and the plane defined by j-k-l

    impr.     proper.
      k             l
      |            /
      j         j-k
     / \       /
    i   l     i
    
    Parameters are given in the order type_i type_j type_k type_l in the 
    impropers section of the parameter file, e.g.
    [impropers] improper type definitions
    *taci  tacj     tack    tacl    forceK     phase
    *-----------------------------------------------------------------
    ?       C       O       ?         10.5     180.
    ?       C       O2      O2        10.5     180.
    Only the central atom j is required.

    Previously, impropers were generated for all atoms connected to three 
    other atoms. A new option is to use explicit definitions of impropers
    in the residue library. This method is used instad of the automatic
    generation if the following appears in the options section of the 
    parameter file

    improper_definition explicit

    In this case, all desired impropers must be specified in the residue 
    library, e.g.

    [impropers]
           -C    N    H   CA
           CA    C    O   +N

    - and + may be used to specify atoms in the previous or the 
    subsequent residue, respectively.
    The recommended place for this section is after the connections section.



## Release 3.51, 99-01-24
    Atom type names are now stored in the topology, after moleule start atoms 
    near the end of the file but before SYBYL atom names are. 
    This may cause confusion in Qprep when writing MOL2 files from a topology
    created with with Qprep v3.31 through 3.50. No problems with Qdyn 
    expected.
    The topology now has a header, describing the files used to generate it
    and the force field used. Remarks may be added in this section. The header
    is composed of lines starting with a keyword. 
    Older topology files may still be read.
    The options section in the parameter file may now contain the keyword name
    to describe the forcefield in the topology.

## Release 3.49, 98-11-04
    Atom types are now strings (currently 8 characters). The strings are used 
    in place of the numeric atom type codes wherever those appear in parameter
    or library files. A new index module 
    encapsulates the code to look up atom type names. Atom types are still 
    integers in the topology. This also means that only the defined atom 
    types are written to the topology, not all 99 of which most were 
    undefined, as earlier.
    The atom types for water can no longer be hard-coded into Qdyn, but is
    now instead written in the topology: The number of atom types is 
    followed by the atom type numbers for OW and HW. If this data is missing,
    the default values (OW=48, HW=11) are used.
    Aliases for atom type names may be defined in the new section atom_aliases
    in the parameters, to assure compatibility with libraries using the older 
    numeric names. Example:
        [atom_aliases]
        48  O.SPC
        Means '48' is another name for the atom type O.SPC (SPC water oxygen)
    Many aliases may be defined for the same atom type. There must not be 
    duplicates of atom type names.

    
## Release 3.41, 98-09-01
    Fixed problem with counting water molecules when generating topology
    from PDB file.

## Release 3.4, 98-06-29
    New library file format, according to example below:
{ALA}               !Residue name
    [info]
        SYBYLtype  RESIDUE     !SYBYL substructure type
    [atoms]
         1 N      30    -0.280 !At. no., name, iac, charge
         2 H      11     0.280 !At. no., name, iac, charge
         3 CA     22     0.000 !At. no., name, iac, charge
         4 CB     24     0.000 !At. no., name, iac, charge
         5 C      21     0.383 !At. no., name, iac, charge
         6 O      40    -0.383 !At. no., name, iac, charge
    [bonds]         !bonds (intra-residue)
         1    2                !connect i -- j
         1    3                !connect i -- j
         3    4                !connect i -- j
         3    5                !connect i -- j
         5    6                !connect i -- j
    [connections]   !how to bond to previous and next
        head       1
        tail       5
    [charge_groups] !charge groups, with switch atom first
          1    2                  !Atom list
          3                       !Atom list
          5    6                  !Atom list
          4                       !Atom list

    Comments initiated by *, ! or # may appear after data or on separate line
    anywhere. The [info], [connections] and [charge_groups] sections are 
    optional. In the absence of charge groups, a default group is constructed
    including all atoms in the residue and with atom 1 as switching atom.

    A PERL script libconvert.pl is available and converts old libraries.


## Release 3.31, 98-06-25
    Qprep can now write SYBYL mol2 files (provided you use a new format 
    parameter file with SYBYL type information) The new writemol2 command 
    writes one or all molecules with or without hydrogens, with or without 
    water. 
    Co-ordinates can now be read from a trajectory file with the new 
    readframe command. A readnext command is also available - guess what it 
    does! All the co-ordinate reading commands now keep track of the co-
    ordinate source. Using this, a heading is generated in mol2 files. It can 
    also be used to generate the a name for a mol2 file automatically 
    (e.g. md_01.tr.17.2.mol2 for trajectory md_10.tr, frame 17, molecule 2). 

## Release 3.3, 98-06-18
    New format of parameter file - no counting lines needed anymore. The line 
    with the number of parameters of a kind should be replaced by the 
    appropriate section heading (enclosed in [] and spelled as below (not 
    case-sensitive)). The sections can appear in any order. Blank lines and 
    comments starting with *,# or ! in column 1 are permitted anywhere in the
    file, as well as comments at the end of each line.
    The force field options can come in any order but must be preceded by the 
    correct key as shown below.
    If you want to create SYBYL mol2 files with Qprep, you need to designate 
    SYBYL atom types (e.g. C.3 for sp3 carbon) to all atom types. This
    four-character string should follow the atomic mass on the line for each
    atom type. Furthermore, each bond type must be assigned a corresponding
    SYBYL bond type (1, ar(omatic), am(ide), 2, 3). This two-character string
    should follow the equilibrium bond length.

    [bonds] bond types definitions
    *iaci iacj   force.c.  dist.    SYBYL
    10   20   700.000   1.090   1

    [angles] angle type definitions
    * iaci iacj iack  forceK angle0
    10   20   10   90.00  106.60

    [torsions] torsion type definitions
    *iaci iacj iack iacl  forceK  #minima phase   #paths
    0   20   22    0   1.400   3.000    0.000    6

    [impropers] improper torsion type definitions
    *iaci iacj forceK   imp0
    21    0  40.000  180.000

    [options] force field options
    vdw_rule    1   ! vdW combination rule ( 1 = geometric / 2 = arithmetic )
    scale_14    1.0 ! electrostatic 1-4 scaling factor

    [atom_types] atom type definitions
    *-iac---Avdw1---Avdw2---Bvdw1----Avdw3---Bvdw2&3---mass----SYBYL-name-old-comment
    10    0.00     0.00    0.00     0.00    0.00     3.000  H    ! HC   -  non-polar H

    [LJ_type2_pairs] type-2 van der Waals interaction atom type pairs
    *iaci iacj
    30   33

## Release 3.21, 98-04-24
    Topology module updated, formatting of topology file changed to make sure 
    there's space between values.

## Release 3.20, 98-04-20
    Rewrote handling of library data with dynamic allocation. Memory usage
    now 3 Mb instead of 50, for a typical 3000-atom system.

## Release 3.14, 98-02-16
    Fixed bug in bond length lookup in hydrogen generation.
    Fixed bug in read co-ordinate file routine.
    Cleaned up write PDB routine (no functional changes).
    Fixed problem with failure to open topology for reading.

## Release 3.13, 98-02-09
    New command parser with short commands and arguments read from same line
    Hydrogens generated with proper bond length.
