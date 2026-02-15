---
slug: sequencing-technologies-overview
title: "Sequencing Technologies: How Modern DNA Sequencing Works"
authors: [river]
tags: [bioinformatics, sequencing, illumina, pacbio, nanopore, genetics, genomics, dna-sequencing]
image: ./imgs/intro.png
---

# Sequencing Technologies: How Modern DNA Sequencing Works

DNA sequencing is the foundation of modern biology and medicine. Whether you're diagnosing rare diseases, studying evolution, or building personalized medicine, sequencing technology enables us to read the genetic code. But how do these machines actually work? This guide provides a brief overview of three major sequencing platforms and explains the mechanisms behind each technology.

<!-- truncate -->

## What is DNA Sequencing?

DNA sequencing is the process of determining the exact order of nucleotides (A, T, G, C) in a DNA molecule. Modern sequencing machines work in two phases:

1. **Library Preparation**: DNA is fragmented, prepared, and attached to a surface or into a chamber
2. **Sequencing Chemistry**: The machine reads the nucleotides one by one (or in groups) and records the sequence

Different sequencing platforms use fundamentally different chemistry and physics to decode the DNA message. Each has strengths and tradeoffs.

## 1. Illumina Sequencing: Reversible Terminators

Illumina is the most widely used sequencing platform globally, powering roughly 90% of the world's sequencing data.

### How Illumina Works: The Mechanism

```
DNA Preparation:
  Original DNA → Fragment (100-500bp) → Add adapters → Denature (single-stranded)

Cluster Generation:
  1. DNA fragments attach to flow cell surface at random positions
  2. PCR amplification on the surface creates ~1000 copies per cluster
     (Cluster = ~1000 identical DNA copies in a tiny area)
  
Sequencing Chemistry:
  For each cycle (300-600 cycles per run):
    
    1. Add nucleotides with fluorescent dyes + reversible terminator
       [ACGT with 4 different fluorescent colors, each with a terminator that blocks]
    
    2. DNA polymerase adds ONE nucleotide to all clusters simultaneously
       All clusters add the same position together
    
    3. Camera takes image and records which fluorescent color at each cluster
       Color = nucleotide identity (A=red, C=blue, G=green, T=yellow)
    
    4. Remove terminator (reversible blocking) → repeat
       All clusters are now ready for next nucleotide

Result: Read sequences of 50-300bp per cluster
```

### Key Characteristics

| Feature | Details |
|---------|---------|
| **Read Length** | 50-300 bp (short reads) |
| **Accuracy** | 99%+ (Q30-Q50 quality scores) |
| **Speed** | Hours to days (24-96 hours typical) |
| **Cost per Million Bases** | Lowest among all platforms |
| **Best For** | Whole genome sequencing, transcriptomics, variant discovery, large projects |
| **Throughput** | 300-1000 million reads per run |

### Visual Mechanism

```
         Cycle 1        Cycle 2        Cycle 3        Cycle N
         -------        -------        -------        -------
         
Flow Cell with DNA clusters:
  
  ●●●●●●●●●●●●●●●●●●●●  (1000 clusters in small area)
  ●●●●●●●●●●●●●●●●●●●●  
  ●●●●●●●●●●●●●●●●●●●●
  
  Step 1: Add colorful fluorescent nucleotides
  ●●●●●●●●●●●●●●●●●●●●  (All clusters incorporate same position)
    ↓
  Step 2: Camera image
  ● = RED  (A added)
  ● = BLUE (C added)
  ● = GREEN (G added)
  ● = YELLOW (T added)
    ↓
  Step 3: Remove terminator blocker
    ↓
  Repeat for next position
```

### Strengths

- **High accuracy**: 99%+ accurate bases (Q30-Q50)
- **Lowest cost**: Most reads per dollar
- **Massive throughput**: Can sequence thousands of samples in parallel
- **Standardized**: Huge ecosystem of tools and reference data

### Limitations

- **Short reads**: Difficult to assemble complex regions
- **Fixed mode**: All clusters read simultaneously (no flexibility for different lengths)
- **Requires PCR**: Potential bias from cluster amplification

---

## 2. PacBio Sequencing: Single Molecule Real-Time (SMRT)

PacBio (Pacific Biosciences) pioneered long-read sequencing. Unlike Illumina, PacBio reads individual DNA molecules in real-time.

### How PacBio Works: The Mechanism

```
DNA Preparation:
  Original DNA → Fragment (10-50kb, much longer than Illumina) → Add SMRTbell adapters
  SMRTbell = circular adapter allowing multiple passes through sequencing
  
Chamber Setup:
  1. DNA molecule loaded into nanoscale well (SMRT cell)
  2. Each well contains: DNA polymerase enzyme + fluorescent dNTPs
  3. DNA polymerase binds to DNA template
  
Sequencing Chemistry (Real-time observation):
  1. DNA polymerase incorporates nucleotides one at a time
     Each nucleotide has a fluorescent dye attached (A, C, G, T = 4 colors)
  
  2. As polymerase adds nucleotide:
     - Camera watches the specific well
     - Detects fluorescent signal IMMEDIATELY
     - Logs which color (nucleotide identity)
  
  3. DNA polymerase continues to next nucleotide
     No cycles or reset needed - continuous motion!
  
  4. Multiple passes: SMRTbell adapter loops template back
     Polymerase can read same DNA multiple times (Circular Consensus Sequencing)
     This improves accuracy through consensus from multiple passes

Result: Long reads of 10-30kb (some >100kb), continuous process
```

### Key Characteristics

| Feature | Details |
|---------|---------|
| **Read Length** | 10-30 kb typical (up to 100+ kb possible) |
| **Accuracy** | 99%+ with CCS (Circular Consensus Sequencing) |
| **Speed** | Hours to days (similar to Illumina) |
| **Cost per Million Bases** | Higher than Illumina, lower than Nanopore |
| **Best For** | Assembly (de novo, diploid), structural variants, haplotyping, repeat regions |
| **Throughput** | ~5-10 million reads per run |

### Visual Mechanism

```
                    SMRT Cell (Nanoscale Well)
                    
    ╔═══════════════════════════════════════════╗
    ║  Fiber optic waveguide                    ║
    ║        ↑  (shines laser)                  ║
    ║        │                                  ║
    ║     ┌──┴─────────────────────────┐       ║
    ║     │  DNA Polymerase Enzyme     │       ║
    ║     │  (attached to well bottom) │       ║
    ║     └──┬─────────────────────────┘       ║
    ║        │                                  ║
    ║  ◄─────┼──────────────────────────────►  ║
    ║  DNA template being read                 ║
    ║  (polymerase moves along it)             ║
    ║        │                                  ║
    ║     [A] [C] [G] [T] [A] [C] [G]         ║
    ║      RED BLUE GREEN YELLOW RED ...       ║
    ║      (fluorescent nucleotides)           ║
    ║                                           ║
    ║  Camera watches: "RED → BLUE → GREEN"   ║
    ║  Output: A → C → G                       ║
    ╚═══════════════════════════════════════════╝
```

### Strengths

- **Long reads**: 10-30kb reads enable better assembly
- **Excellent for repeats**: Can read through repetitive regions
- **Real-time sequencing**: Flexibility to adjust strategies mid-run
- **Multiple passes**: CCS improves accuracy to 99%+
- **Methylation detection**: Can directly detect modified bases

### Limitations

- **Lower throughput**: Fewer molecules sequenced per run vs Illumina
- **Higher cost per base**: More expensive than Illumina for same amount of data
- **Shorter than Nanopore**: Nanopore can produce longer reads (though less accurate)

---

## 3. Oxford Nanopore Sequencing: Ionic Current

Oxford Nanopore uses a completely different approach—reading DNA by measuring electrical signals through tiny protein pores.

### How Oxford Nanopore Works: The Mechanism

```
DNA Preparation:
  Original DNA → Fragment (can be very long, 5-100+ kb) → Add adapter with motor protein
  Motor protein = molecular machine that controls DNA movement
  
Nanopore Setup:
  1. Biological nanopores (α-hemolysin) inserted into synthetic membrane
  2. Each pore has: voltage gradient across membrane + electrical detector
  3. DNA polymerase (motor protein) attached to DNA molecule
  
Sequencing Chemistry (Ionic current detection):
  1. Motor protein threads DNA through nanopore one nucleotide at a time
     
  2. As DNA passes through pore:
     - Each nucleotide has unique ionic current signature
     - Current measured in picoamperes (pA)
     - A, C, G, T produce slightly different blockade currents
     
  3. Machine records current over time
     Changes in current = changes in nucleotides
  
  4. Very fast process: nucleotides pass through at ~1000 nt/second
     Produces ~100-200 bases per second output (very fast!)

Result: Very long reads (5-100+ kb), ultra-long possible (500kb+)
       Lower accuracy than Illumina/PacBio but compensated by length
```

### Key Characteristics

| Feature | Details |
|---------|---------|
| **Read Length** | 5-100+ kb typical (can exceed 500kb) |
| **Accuracy** | 85-99%+ (depending on base-calling model) |
| **Speed** | Real-time output (hours to days) |
| **Cost per Million Bases** | Lowest when considering per-read cost, high per-million cost due to fewer reads |
| **Best For** | Ultra-long assemblies, structural variants, real-time applications, mobile/field sequencing |
| **Throughput** | 0.5-2 million reads per run (lower than others) |

### Visual Mechanism

```
         Nanopore Sequencing: Electrical Detection
         
         ┌─────────────────────────────────────────┐
         │      SYNTHETIC MEMBRANE (lipid bilayer) │
         │                                         │
         │                                         │
         │     ╔═══════════════════════════════╗  │
         │     ║  Motor Protein (DNA Polymerase) ║  │
         │     ║  (controls DNA movement)        ║  │
         │     ║                                 ║  │
         │     ║        ▼ DNA strand             ║  │
         │     ║    ┌─────────────────────┐     ║  │
         │     ║    │ DNA sequence: ACGTAG│ ◄───╫──┤ To voltage source
         │     ║    └─────────────────────┘     ║  │
         │     ║        ▼                       ║  │
         │     ║  ◄─ Nanopore (α-hemolysin)  ║  │ 
         │     ║     A → 120 pA (current)      ║  │
         │     ║     C → 95 pA                 ║  │
         │     ║     G → 110 pA                ║  │
         │     ║     T → 105 pA                ║  │
         │     ║                                 ║  │
         │     ╚═══════════════════════════════╝  │
         │                                         │
         │  Electrical current measured:           │
         │  Voltage applied across membrane        │
         │  Detector records current changes       │
         │                                         │
         └─────────────────────────────────────────┘
         
         Ionic current graph over time:
         
         Current
         (pA)
         ▲
         │    ╱╲    ╱╲    ╱╲    ╱╲
      120├───╱  ╲──╱  ╲──╱  ╲──╱  ╲─ A
         │      
      110├        ╱╲    ╱╲    
         │       ╱  ╲  ╱  ╲           G
         │      
      105├            ╱╲
         │           ╱  ╲              T
         │          ╱    ╲
      95 ├─────────╱──────╲────────── C
         │
         └─────────────────────────────► Time
```

### Strengths

- **Ultra-long reads**: Best read lengths of any sequencing platform
- **Portable**: MinION device is pocket-sized, can sequence in the field
- **Real-time data**: Can stop run early if needed, or analyze data live
- **No PCR bias**: Single molecule sequencing, no amplification
- **Methylation detection**: Can detect methylated bases in DNA

### Limitations

- **Lower accuracy**: Base-calling accuracy lower than Illumina/PacBio (though improving)
- **Lower throughput**: Fewer molecules sequenced per run
- **Higher per-base cost**: For raw bases, expensive per megabase
- **Requires expertise**: Best results need good library prep and troubleshooting

---

## Comparison: Which Technology to Choose?

### Feature Comparison Table

| Feature | Illumina | PacBio | Oxford Nanopore |
|---------|----------|--------|-----------------|
| **Read Length** | 50-300 bp | 10-30 kb | 5-100+ kb |
| **Accuracy** | 99%+ | 99%+ (with CCS) | 85-99%+ |
| **Cost per Mb** | Lowest | Medium | Lower per-base if high throughput |
| **Speed** | Hours-days | Hours-days | Real-time |
| **Throughput** | ~1 billion reads | ~5-10 million reads | ~1 million reads |
| **Setup Cost** | High | High | Low (MinION: ~$1000) |
| **Best Applications** | Population studies, WGS, transcriptomics | Assembly, structural variants | Ultra-long assembly, portable |

### Decision Guide

**Choose Illumina if you need:**
- Maximum accuracy (>99%)
- Lowest cost per base
- High throughput (many samples)
- Short variant discovery
- Standard DNA/RNA analysis

**Choose PacBio if you need:**
- Long reads for assembly
- Complex region resolution
- Structural variant discovery
- Haplotype information
- Repetitive region analysis

**Choose Oxford Nanopore if you need:**
- Ultra-long reads (50+ kb)
- Field sequencing capability
- Real-time data
- Methylation information
- Rapid turnaround (portable)

---

## The Future of Sequencing

Sequencing technology continues to evolve. Current trends include:

- **Increased read length**: Nanopore exploring >1Mb reads
- **Improved accuracy**: Machine learning models reducing error rates
- **Cost reduction**: Per-base sequencing costs continuing downward (following Moore's Law trend)
- **Integration**: Combining multiple technologies in single workflows
- **Real-time analysis**: Streaming data processing instead of batch analysis
- **In-field deployment**: Portable sequencing becoming routine in clinics and remote locations

---

## Key Takeaways

1. **Illumina dominates** through high accuracy, low cost, and massive throughput—ideal for large population studies and standardized workflows

2. **PacBio excels** at long-read accuracy through real-time single-molecule sequencing—perfect for assembly and complex variant discovery

3. **Nanopore pioneers** true ultra-long-read sequencing with portable devices—transforming rapid diagnostics and field biology

4. **Different problems need different tools**: The best sequencing platform depends on your research question, budget, and timeline

5. **Hybrid approaches**: Modern genomics often combines multiple platforms—Illumina for accuracy, PacBio for assembly, Nanopore for long scaffolds

---

## Additional Resources

- [Illumina Sequencing Technology Overview](https://www.illumina.com/techniques/sequencing/ngs-technology.html)
- [PacBio SMRT Sequencing Guide](https://www.pacb.com/technology/smrt-sequencing/)
- [Oxford Nanopore Technology Guide](https://nanoporetech.com/application/whats-nanopore-sequencing)
- [Sequencing Technology Benchmarks](https://www.nature.com/articles/nrg.2016.49)
- [Real-time Nanopore Sequencing](https://www.nature.com/articles/nbt.4038)
