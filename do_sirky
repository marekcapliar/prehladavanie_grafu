import tkinter as tk


def create_towns(town_size = 5):
    ids = {}
    for i in vrcholy:
        temp = canvas.create_oval(vrcholy[i][0] - town_size, vrcholy[i][1] - town_size, vrcholy[i][0] + town_size, vrcholy[i][1] + town_size, fill="green", outline="green")
        canvas.create_text(vrcholy[i][0], vrcholy[i][1] - 15, text=i)
        ids[i] = temp
    return ids


def create_connections():
    global hrany
    connections = {}
    for i in hrany:
        mesto1, mesto2 = i[0], i[1]
        x1 = vrcholy[i[0]][0]
        y1 = vrcholy[i[0]][1]
        x2 = vrcholy[i[1]][0]
        y2 = vrcholy[i[1]][1]
        temp = canvas.create_line(x1, y1, x2, y2, fill='red')
        connections = add_value_to_towns(mesto1, mesto2, temp, connections)
    return connections


def add_value_to_towns(town1, town2, value, dictionary):
    temp_dict = {town2: value}
    dictionary[town1] = dictionary.get(town1, {})
    dictionary[town1].update(temp_dict)
    temp_dict = {town1: value}
    dictionary[town2] = dictionary.get(town2, {})
    dictionary[town2].update(temp_dict)
    return dictionary


def do_town_distances():
    global hrany, vrcholy
    town_distances = {}
    for i in hrany:
        mesto1, mesto2 = i[0], i[1]
        distance = ((vrcholy[mesto1][0] - vrcholy[mesto2][0])**2 + (vrcholy[mesto1][1] - vrcholy[mesto2][1])**2)**(1/2)
        town_distances = add_value_to_towns(mesto1, mesto2, distance, town_distances)
    return town_distances


def into_the_deep(start_town: str = '', visited: list = [], end_town: str = ''):
    visited.append(start_town)
    canvas.itemconfig(town_ids[start_town], fill = 'red')
    for i in town_distances[start_town]:
        if i not in visited:
            canvas.itemconfig(connection_ids[start_town][i], fill = "purple")
            into_the_deep(i, visited, end_town)
    return visited


def into_the_width(start_town: str = "Bratislava", visited: list = [], processing_towns: list = []):
    processing_towns.append(start_town)
    while len(processing_towns) > 0:
        current_town = processing_towns[0]
        canvas.itemconfig(town_ids[current_town], fill = 'red')
        processing_towns.pop(processing_towns.index(current_town))
        for i in town_distances[current_town]:
            if i not in visited:
                processing_towns.append(i)
                visited.append(i)
    return visited


h = open("hrany.txt", 'r', encoding="UTF-8")
v = open("vrcholy.txt", 'r', encoding="UTF-8")

hrany = h.readlines()
hrany = [i.strip().split(';') for i in hrany]

vrcholy = v.readlines()
vrcholy = [i.strip().split(';') for i in vrcholy]
vrcholy = {i[0]: (int(i[1]), int(i[2])) for i in vrcholy}

root = tk.Tk()

canvas = tk.Canvas(root, width=1000, height=600, bg='pink')
canvas.pack()

connection_ids = create_connections()
town_ids = create_towns()

town_distances = do_town_distances()
# every_town = into_the_deep('Snina')
every_town = into_the_width("Bratislava")

root.mainloop()
