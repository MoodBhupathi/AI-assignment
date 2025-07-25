class RabbitLeap:
    def __init__(self, state=None):
        self.state = state if state else "EEE_WWW"
        self.goal = "WWW_EEE"

    def goal_test(self):
        return self.state == self.goal

    def move_gen(self):
        neighbors = []
        s = list(self.state)
        empty = s.index('_')

        # E moves only right (towards bigger index)
        # W moves only left (towards smaller index)

        # 1-step moves:
        # E moves right if empty is right by 1
        if empty - 1 >= 0 and s[empty - 1] == 'E':
            new_s = s[:]
            new_s[empty], new_s[empty - 1] = new_s[empty - 1], '_'
            neighbors.append(''.join(new_s))

        # W moves left if empty is left by 1
        if empty + 1 < len(s) and s[empty + 1] == 'W':
            new_s = s[:]
            new_s[empty], new_s[empty + 1] = new_s[empty + 1], '_'
            neighbors.append(''.join(new_s))

        # 2-step jumps:
        # E jumps right over W (empty two right of E)
        if empty - 2 >= 0 and s[empty - 2] == 'E' and s[empty - 1] == 'W':
            new_s = s[:]
            new_s[empty], new_s[empty - 2] = new_s[empty - 2], '_'
            neighbors.append(''.join(new_s))

        # W jumps left over E (empty two left of W)
        if empty + 2 < len(s) and s[empty + 2] == 'W' and s[empty + 1] == 'E':
            new_s = s[:]
            new_s[empty], new_s[empty + 2] = new_s[empty + 2], '_'
            neighbors.append(''.join(new_s))

        return neighbors


def bfs_solve_rabbit():
    start_puzzle = RabbitLeap()
    queue = [[start_puzzle.state]]
    visited = set()

    while queue:
        path = queue.pop(0)
        current = path[-1]

        if current == start_puzzle.goal:
            return path

        if current not in visited:
            visited.add(current)
            puzzle = RabbitLeap(current)
            for nxt in puzzle.move_gen():
                if nxt not in visited:
                    queue.append(path + [nxt])
    return None


# Run and print solution:
solution = bfs_solve_rabbit()
if solution:
    print(f"Solution found in {len(solution)-1} moves:")
    for state in solution:
        print(state)
else:
    print("No solution found.")
