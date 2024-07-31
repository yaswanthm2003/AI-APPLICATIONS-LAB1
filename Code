import random

class VacuumCleanerWorld:
    def __init__(self, size=3, dirt_count=3):
        self.size = size
        self.grid = [['clean' for _ in range(size)] for _ in range(size)]
        self.dirt_positions = self._place_dirt(dirt_count)
        self.agent_position = self._random_position(exclude=self.dirt_positions)
        self.initial_position = self.agent_position
        self.cleaned_dirt = 0
        self.total_dirt = dirt_count
        self.path = [self.agent_position]

    def _random_position(self, exclude=[]):
        positions = [(x, y) for x in range(self.size) for y in range(self.size) if (x, y) not in exclude]
        return random.choice(positions)

    def _place_dirt(self, dirt_count):
        dirt_positions = []
        for _ in range(dirt_count):
            pos = self._random_position(exclude=dirt_positions)
            dirt_positions.append(pos)
            self.grid[pos[0]][pos[1]] = 'dirty'
        return dirt_positions

    def get_sensors(self):
        x, y = self.agent_position
        status = self.grid[x][y]
        return {
            'position': self.agent_position,
            'status': status
        }

    def move_agent(self, direction):
        x, y = self.agent_position
        if direction == 'up' and x > 0: x -= 1
        elif direction == 'down' and x < self.size - 1: x += 1
        elif direction == 'left' and y > 0: y -= 1
        elif direction == 'right' and y < self.size - 1: y += 1
        else: return "Move not possible"

        self.agent_position = (x, y)
        self.path.append((x, y))
        return f"Moved {direction}"

    def suck(self):
        x, y = self.agent_position
        if self.grid[x][y] == 'dirty':
            self.grid[x][y] = 'clean'
            self.cleaned_dirt += 1
            return "Cleaned the room"
        return "Room already clean"

    def get_status(self):
        sensors = self.get_sensors()
        status = {
            'position': self.agent_position,
            'status': sensors['status'],
            'cleaned_dirt': self.cleaned_dirt,
            'total_dirt': self.total_dirt,
            'path': self.path
        }
        return status

    def trace_back(self):
        path_back = self.path[::-1]
        for position in path_back[1:]:
            self.agent_position = position
            print(f"Moved back to {position}")
        self.path = [self.initial_position]

def main():
    world = VacuumCleanerWorld()

    while True:
        status = world.get_status()
        print("Agent position:", status['position'])
        print("Room status:", status['status'])
        print("Cleaned dirt:", status['cleaned_dirt'])
        print("Total dirt:", status['total_dirt'])
        print("Path covered:", status['path'])
        print()

        if status['cleaned_dirt'] == status['total_dirt'] and world.agent_position == world.initial_position:
            print("Congratulations! All rooms are clean and the agent is back to the initial position!")
            break
        elif status['cleaned_dirt'] == status['total_dirt']:
            print("All rooms are clean. Now returning to the initial position.")
            world.trace_back()
            if world.agent_position == world.initial_position:
                print("Congratulations! All rooms are clean and the agent is back to the initial position!")
                break

        action = input("Enter action (move up/down/left/right, suck): ")
        if action.startswith('move '):
            direction = action.split()[1]
            result = world.move_agent(direction)
        elif action == 'suck':
            result = world.suck()
        else:
            result = "Invalid action!"

        print(result)

if __name__ == "__main__":
    main()
