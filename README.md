import tkinter as tk

WIDTH, HEIGHT = 600, 400
cell = {'size': 20, 'color': 'light grey'}
COLS, ROWS = WIDTH//cell['size'], HEIGHT//cell['size']
data = [[False for x in range(COLS)] for y in range(ROWS)]

win = tk.Tk()
win.title('LifeGame')



text = tk.StringVar()
update = 0
text.set(update)
label = tk.Label(win, textvariable=text,  font=('IPAex Gothic', '24'))
label.pack()


cv = tk.Canvas(win, width=WIDTH, height=HEIGHT, bg='brown')
cv.pack()



def draw_grid():
    for x in range(COLS):
        x1 = x * cell['size']
        cv.create_line(x1, 0, x1, HEIGHT, fill='white')
    for y in range(ROWS):
        y1 = y * cell['size']
        cv.create_line(0, y1, WIDTH, y1, fill='white')



def place_cells(e):
    draw_grid()

    x, y = e.x // cell['size'], e.y // cell['size']

    if data[y][x]:
        cv.delete('current')
        data[y][x] = False

    else:
        x1, y1 = x * cell['size'], y * cell['size']
        cv.create_oval(x1, y1, x1+cell['size'], y1+cell['size'], fill=cell['color'])
        data[y][x] = True


cv.bind('<Button>', place_cells)



def check_cells(x, y):

    tbl = [(-1, -1), (0, -1), (1, -1), (1, 0), (1, 1), (0, 1), (-1, 1), (-1, 0)]
    cnt = 0

    for t in tbl:
        xx, yy = x + t[0], y + t[1]
        if not 0 <= xx < COLS or not 0 <= yy < ROWS:
            continue
        if data[yy][xx]:
            cnt += 1

    if cnt == 3:
        return True
    if data[y][x]:
        if 2 <= cnt <= 3:
            return True
        return False
    return data[y][x]



def next_turn(e):
    global data
    global update

    data2 = [[check_cells(x, y) for x in range(COLS)] for y in range(ROWS)]
    data = data2
    update += 1
    text.set(update)
    draw()


win.bind('<Return>', next_turn)  #When Enter is pressed


#Draw a cell.
def draw():
    cv.delete('all')
    for y in range(ROWS):
        for x in range(COLS):
            if data[y][x]:
                x1, y1 = x * cell['size'], y * cell['size']
                cv.create_oval(x1, y1, x1+cell['size'], y1+cell['size'], fill=cell['color'])


win.mainloop()
