# LoadPlacer

A Windows Forms application for optimizing the placement and distribution of loads (pallets, cargo) into containers using 3D bin packing algorithms.
This project developed to address a specific need arising from my mother's profession.

## Overview

LoadPlacer helps logistics and warehouse professionals efficiently pack items into containers by automatically calculating optimal placement positions. The application uses the EB-AFIT (Extreme-point Based Approximate Fit) algorithm to maximize space utilization.

## Features

- **Single Container Packing**: Pack multiple items into a single container with optimal positioning
- **Multi-Container Distribution**: Distribute items across multiple containers when a single container is insufficient
- **3D Bin Packing**: Utilizes industry-standard 3D packing algorithms for efficient space utilization
- **Visual Interface**: Windows Forms UI for easy interaction and visualization
- **Item Grouping**: Automatically groups similar items for more efficient packing calculations

## Requirements

- Windows OS
- .NET 8.0 SDK or later
- Visual Studio 2022 (recommended)

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/cihangiryaman/LoadPlacer.git
   ```

2. Open `LoadPlacer.sln` in Visual Studio

3. Restore NuGet packages:
   ```bash
   dotnet restore
   ```

4. Build and run the solution

## Algorithm

The application uses the **EB-AFIT (Extreme-point Based Approximate Fit)** algorithm, which is well-suited for 3D bin packing problems. This algorithm:

- Identifies extreme points (potential placement positions) within the container
- Evaluates items for best fit at each position
- Optimizes for maximum space utilization

## Project Structure

```
LoadDistributor/
├── LoadPlacer/                    # Main application
│   ├── Business/                  # Business logic
│   │   └── Distributor.cs         # Core packing/distribution logic
│   ├── Data/                      # Data models
│   │   ├── Abstract/              # Interfaces
│   │   │   ├── IContainer.cs
│   │   │   └── ILoad.cs
│   │   └── Concrete/              # Implementations
│   │       ├── Load.cs
│   │       └── PackingResult.cs
│   ├── UI/                        # User interface
│   │   ├── Forms/
│   │   │   └── MainForm.cs
│   │   └── UserControls/
│   │       └── ContainerUC.cs
│   └── Program.cs                 # Application entry point
├── Test/                          # Unit tests (NUnit)
│   └── CategorizeLoadsTest.cs
└── LoadPlacer.sln                 # Solution file
```

## Usage

### Basic Packing

```csharp
using CromulentBisgetti.ContainerPacking.Entities;
using LoadPlacer.Business;

// Define container dimensions (Length, Width, Height)
Container container = new Container(0, 280, 100, 100);

// Create items to pack (ID, Dim1, Dim2, Dim3, Quantity)
List<Item> items = new List<Item>
{
    new Item(0, 40, 30, 15, 1),
    new Item(1, 60, 25, 20, 2),
    new Item(2, 50, 20, 20, 3)
};

// Initialize distributor and pack items
Distributor distributor = new Distributor();
List<Item> placedItems = distributor.PlaceToSingle(items, container);

// Access placement coordinates
foreach (var item in placedItems)
{
    Console.WriteLine($"Item placed at: ({item.CoordX}, {item.CoordY}, {item.CoordZ})");
}
```

### Multi-Container Distribution

```csharp
// Distribute items across multiple containers
short containerCount = 3;
List<ContainerPackingResult> results = distributor.PlaceToMultiple(items, container, containerCount);

foreach (var result in results)
{
    Console.WriteLine($"Container {result.ContainerID}: {result.AlgorithmPackingResults.Count} packing results");
}
```

## Data Models

### Load Properties

| Property | Type | Description |
|----------|------|-------------|
| Id | int | Unique identifier |
| Name | string | Load name (default: "Paletli Yük") |
| Width | short | Width dimension |
| Height | short | Height dimension |
| Length | short | Length dimension |
| IsTough | bool | Whether the load is sturdy/stackable |
| LDM | float? | Loading meter value |
| XPoint, YPoint, ZPoint | short | Placement coordinates |

### Container Properties

| Property | Type | Description |
|----------|------|-------------|
| Length | short | Container length |
| Width | short | Container width |
| Height | short | Container height |

## Dependencies

- [CromulentBisgetti.ContainerPacking](https://github.com/davidmchapman/3DContainerPacking) - 3D bin packing library

## Running Tests

```bash
cd Test
dotnet test
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -m 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Open a Pull Request

## License

This project is available for use under standard open source terms.

## Acknowledgments

- [3DContainerPacking](https://github.com/davidmchapman/3DContainerPacking) by David Chapman for the container packing algorithms
