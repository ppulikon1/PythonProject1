import json
import sys

class GameState:
    def __init__(self, map_file):
        try:
            with open(map_file, 'r') as f:
                self.map_data = json.load(f)
        except FileNotFoundError:
            print(f"Error: file '{map_file}' not found.")
            sys.exit(1)
        except json.JSONDecodeError:
            print(f"Error: file '{map_file}' contains invalid JSON.")
            sys.exit(1)
        self.current_room = 0
        self.previous_room = None

    def get_room(self):
        return self.map_data['rooms'][self.current_room]

    def get_exit_list(self):
        exit_list = "Exits: "
        for direction, destination in self.get_room()['exits'].items():
            exit_list += f"{direction} ({destination}), "
        return exit_list[:-2]
    def set_previous_room(self, room):
        self.previous_room = room['name'] # or room['id'] if you want to use the room ID
    def get_previous_room(self):
        return self.previous_room

class PlayerState:
    def __init__(self):
        self.inventory = []

    def add_item(self, item):
        self.inventory.append(item)

    def remove_item(self, item):
        if item in self.inventory:
            self.inventory.remove(item)

    def get_inventory(self):
        if len(self.inventory) == 0:
            return "You have no items."
        else:
            return "Inventory: " + ", ".join(self.inventory)

# Check if the user provided the filename of the map as an argument
if len(sys.argv) != 2:
    print("Usage: python3 adventure.py <map_file>")
    sys.exit(1)

game_state = GameState(sys.argv[1])
player_state = PlayerState() 

def start_game():
    previous_room_name = None
    #game loop
    while True:
        # Print the current room's description
        room = game_state.get_room()
        if previous_room_name is not None and room['name'] == previous_room_name:
               None
        else:
            print(room['name'])
            print(room['desc'])
            # Print the available exits
            print("")
            print(game_state.get_exit_list())
            if 'items' in room:
                if room['items']:
                    print("")
                    print("Items in the room: " + ", ".join(room['items']))
        previous_room_name = room['name']
        game_state.set_previous_room(room)

        # Get user input
        print("")
        command = input("> What would you like to do?")
        print("")

        # Process user input
        if command == "quit":
            print("Thanks for playing!")
            sys.exit(0)
        elif command =="look":
            print(room['name'])
            print(room['desc'])
            # Print the available exits
            print("")
            print(game_state.get_exit_list())
            if 'items' in room:
                if room['items']:
                    print("")
                    print("Items in the room: " + ", ".join(room['items']))
        elif command.startswith("go ") or command.startswith("go"):
            item = command[3:]
            try:
                if item in room['exits']:
                    game_state.current_room = room['exits'][item]
                elif len(item)==0:
                    print("Sorry, you need to 'go' somewhere.")
                else :
                    print("There is no room with that name! ")
            except KeyError:
                    print("There was an error accessing the items in this room.")
            
        elif command.startswith("get "):
            item = command[4:]
            try:
                if item in room['items']:
                    player_state.add_item(item)
                    room['items'].remove(item)
                    print(f"You pick up the {item}.")
                else:
                    print("That item is not here.")
            except KeyError:
                    print("There was an error accessing the items in this room.")
    
        elif command.startswith("drop "):
            item = command[5:]
            try:
                if item in player_state.inventory:
                    player_state.remove_item(item)
                    room['items'].append(item)
                    print(f"You drop the {item}.")
                else:
                    print("You do not have that item.")
            except KeyError:
                print("There was an error accessing your inventory.")
        elif command.startswith("inventory"):
            item = command[5:]
            try:
                inventory = player_state.get_inventory()
                if len(inventory) == 0:
                    print("You do not have any items in inventory")
                else:
                    print(player_state.get_inventory())
            except KeyError:
                print("There was an error accessing your inventory.")
        else:
            print("Invalid command.")

#menu for game
print(" Start \n Help \n Quit ")
command = input("> Select one Option ? ")
if command =="Start" or command == "start":
    print("")
    start_game()
elif command == "Quit" or command =="quit":
    print("Thanks for playing!")

