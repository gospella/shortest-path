# shortest-path

#Ввод данных
n = int(input())
gr = {}
for x in range(n):
    tmp = input().split(" ")
    tm = {}
    for y in range(n):
        if(tmp[y] == '0'): continue
        tm[str(y)] = int(tmp[int(y)])
    gr[str(x)] = tm

#print(gr)

#Стартовое значение
startPoint = '0'
minResults = []

#paths: Поиск путей между двумя вершинами
#Возвращает список возможных решений
def paths(gr, frm, to, path_len=0, visited=None):
    if frm == to:
        return [path_len]
    visited = visited or []
    result = []
    # Анализ всех вершин, до котрых от frm есть прямой путь
    for point, length in gr[frm].items():
        if point in visited:
            continue
        visited.append(point)
        for sub_path in paths(gr, point, to, path_len + length, visited[:]):
            result.append(sub_path)
    #print(result)
    return result

#Обход всех вершин графа (в глубину)
#После очередного варианта прохода всех вершин в minResults записывается длина этого пути
def traverse(gr, frm, visited=None, prevMin=0):
    #print("prev" + str(prevMin))
    visited = visited or []
    if (frm not in visited): visited.append(frm)
    # Анализ всех вершин, до котрых от frm есть прямой путь
    for point in gr:
        if point[0] in visited:
            continue
        minPthFrmToPoint = 0
        if(point[0] in gr[frm]):
            minPthFrmToPoint = gr[frm][point[0]]
        else:
            minPthFrmToPoint = min(paths(gr,frm, point[0]))
        pathMin = prevMin + minPthFrmToPoint
        if(len(minResults) and pathMin >= minResults[0]):
            #print("bad:" + frm + " to " + point[0] + ", curmin:" + str(pathMin))
            continue
        #print(frm + " to " + point[0] + ", curmin:" + str(pathMin))
        traverse(gr, point[0], visited[:], pathMin)
    else:
        if (len(visited) == n):
            minPthFrEndtoStart = 0
            if (startPoint in gr[visited[-1]]):
                minPthFrEndtoStart = prevMin + gr[visited[-1]][startPoint]
            else:
                minPthFrEndtoStart = prevMin + min(paths(gr, visited[-1], startPoint))
            minResults.append(minPthFrEndtoStart)
    return

traverse(gr, startPoint)

print(minResults)
print(min(minResults))
