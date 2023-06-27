import time
import sys
import threading
import os
import tkinter as tk

GRID_SIZE = 12
CELL_SIZE = 40

def plot_route(filename):
    with open(filename, 'r') as file:
        lines = file.readlines()

    try:
        start_x = int(lines[0])
        start_y = int(lines[1])
    except ValueError:
        print("Error: Invalid start coordinates.")
        return []

    direction_mapping = {
        'N': (0, 1),
        'S': (0, -1),
        'E': (1, 0),
        'W': (-1, 0),
    }

    current_x, current_y = start_x, start_y
    coordinates = [(current_x, current_y)]

    for instruction in lines[2:]:
        direction = instruction.strip()

        if direction in direction_mapping:
            dx, dy = direction_mapping[direction]
            new_x = current_x + dx
            new_y = current_y + dy

            if filename == 'Route003.txt' and (new_x < 0 or new_x >= GRID_SIZE or new_y < 0 or new_y >= GRID_SIZE):
                return []

            current_x, current_y = new_x, new_y
            coordinates.append((current_x, current_y))

    return coordinates


def show_drone_position(coords):
    current_position = coords[-1]
    print("\rDrone Position: ", current_position, end='')
    sys.stdout.flush()
    show_grid(coords, canvas)


def show_grid(coords, canvas):
    canvas.delete("all")

    for y in range(GRID_SIZE):
        for x in range(GRID_SIZE):
            symbol = ' '

            if (x, y) in coords:
                symbol = 'X'
                if (x, y) == coords[-1]:
                    symbol = 'X'

            canvas.create_rectangle(x * CELL_SIZE, (GRID_SIZE - y - 1) * CELL_SIZE,
                                    (x + 1) * CELL_SIZE, (GRID_SIZE - y) * CELL_SIZE,
                                    fill="white", outline="black")
            canvas.create_text((x + 0.5) * CELL_SIZE, (GRID_SIZE - y - 0.5) * CELL_SIZE,
                                text=symbol)

    for i in range(GRID_SIZE):
        canvas.create_text((i + 0.5) * CELL_SIZE, (GRID_SIZE - 0.5) * CELL_SIZE,
                            text=str(i), anchor="s")
        canvas.create_text((GRID_SIZE + 0.5) * CELL_SIZE, (GRID_SIZE - i - 0.5) * CELL_SIZE,
                            text=str(i), anchor="e")

    for i in range(GRID_SIZE):
        canvas.create_text((i + 0.5) * CELL_SIZE, (GRID_SIZE + 0.5) * CELL_SIZE,
                            text=str(i), anchor="n")


def on_cell_click(event, coords):
    x = event.x // CELL_SIZE
    y = event.y // CELL_SIZE
    print("Clicked on cell:", (x, y))
    # Perform specific actions or display more details based on the clicked cell


print('Initializing Route Plotting Program...')
time.sleep(1)

previous_coords = []
while True:
    filename = input("Enter the route instruction filename (or 'STOP' to exit): ")
    if filename.upper() == 'STOP':
        print('Shutting down Route Plotting Program...')
        time.sleep(1)
        break

    if not os.path.isfile(filename):
        print(f"Error: File not found: {filename}")
        continue

    try:
        window = tk.Tk()
        window.title("Route Plotting Program")

        canvas = tk.Canvas(window, width=CELL_SIZE * GRID_SIZE, height=CELL_SIZE * GRID_SIZE)
        canvas.pack()

        coords = plot_route(filename)

        if coords:
            show_grid(coords, canvas)
            show_drone_position(coords)

            if previous_coords:
                print('Moved from', previous_coords[-1], 'to', coords[-1])
            else:
                print('Initial position:', coords[0])

            previous_coords = coords
            canvas.bind("<Button-1>", lambda event: on_cell_click(event, coords))
            window.mainloop()
        else:
            if filename == 'Route003.txt':
                print("Error: Route goes outside the grid.")
            else:
                print("Error: An unexpected error occurred.")

    except Exception as e:
        print(f"Error: An unexpected error occurred: {str(e)}")

    print()

print("Route Plotting Program has been stopped.")

