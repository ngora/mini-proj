<h1><strong>Metal Ion Modeling with MCPB and MCPB.py(Amber)</strong></h1>
<p><em>based on tutorial form http://ambermd.org/tutorials/advanced/tutorial20/index.htm</em></p>

<p>Modeling histone acetyltransferase 1 (2GIV), which has a Zn-CCCH (H as HID) cluster (with Center ID as 3) in the metal site (<em> due to an example on http://ambermd.org/tutorials/advanced/tutorial20/ZAFF.htm)</em>.</p>

<p><strong>PDB files:</strong></p>
<em>2GIV.pdb</em>

<h2>1. Clean up pdb file and add hydrogen atoms</p></h2>
<p>leave structural information of pdb:</p>
awk '$1=="ATOM" || $1=="HETATM" || $1=="TER" || $1=="END"' 2GIV.pdb > 2GIV_clean1.pdb

<p>Manually, we delete the residues between ALY101 to PRO274 and the ligand ACO in the pdb
2GIV_clean1.pdb and save it as  2GIV_clean2.pdb</p>


