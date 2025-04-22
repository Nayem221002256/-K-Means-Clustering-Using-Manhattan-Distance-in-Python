# -K-Means-Clustering-Using-Manhattan-Distance-in-Python
sabbir hosen mamun
 import random
import numpy as np

# Constants
GRID_SIZE = 50
NUM_POINTS = 100
NUM_CLUSTERS = 10
MAX_ITERATIONS = 10

# Manhattan Distance Function
def manhattan(p1, p2):
    return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])

# Generate Random Points
points = [(random.randint(0, GRID_SIZE-1), random.randint(0, GRID_SIZE-1)) for _ in range(NUM_POINTS)]

# Generate Initial Cluster Centers
centers = [(random.randint(0, GRID_SIZE-1), random.randint(0, GRID_SIZE-1)) for _ in range(NUM_CLUSTERS)]

# K-Means with Manhattan Distance
def kmeans(points, centers, iterations=MAX_ITERATIONS):
    for _ in range(iterations):
        # Assign each point to the nearest cluster
        clusters = {i: [] for i in range(len(centers))}
        for point in points:
            distances = [manhattan(point, center) for center in centers]
            closest_center = distances.index(min(distances))
            clusters[closest_center].append(point)

        # Recalculate centers as mean (using integer average)
        for i in clusters:
            if clusters[i]:  # Avoid division by zero
                xs, ys = zip(*clusters[i])
                new_center = (sum(xs)//len(xs), sum(ys)//len(ys))
                centers[i] = new_center
    return centers, clusters

# Run the algorithm
final_centers, final_clusters = kmeans(points, centers)

# Create a visual grid
grid = [['.' for _ in range(GRID_SIZE)] for _ in range(GRID_SIZE)]

# Mark all points with '*'
for cluster_id, cluster_points in final_clusters.items():
    for x, y in cluster_points:
        grid[y][x] = '*'

# Mark cluster centers with 'C' (overwrites points if overlap)
for cx, cy in final_centers:
    grid[cy][cx] = 'C'

# Print the grid (Y axis reversed to match Cartesian layout)
print("\nClustered 2D Grid (C = Cluster Center, * = Point):\n")
for row in reversed(grid):
    print(' '.join(row))
