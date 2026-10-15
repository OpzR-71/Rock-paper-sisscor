import random
import time
import serial 

arduino = serial.Serial("/dev/cu.usbserial-A5069RR4", 9600, timeout = 1)
time.sleep(2)  # Allow Arduino to reset

def show_lcd(line1, line2=""):
    arduino.write(f"{str(line1)[:16]}\n{str(line2)[:16]}\n".encode("utf-8"))


strategy = random.choice(["sunshine_lolipops", "EMT", "sunshine_lolipops", "EMT", "sunshine_lolipops", "EMT", "sunshine_lolipops", "EMT",])

round = 0

wins = 0
ties = 0
losses = 0


 
while round < 10:
    players_last_move = ""
    bots_last_move = ""
    num = random.randint(1, 10)
    round += 1
    show_lcd("your move? r p s?")
    players_move = input("Enter your move (r for rock, p for paper, s for scissors): ")
    while players_move not in ["r", "p", "s"]:
        show_lcd("try again")
        players_move = input("r, p or s: ")
    if strategy == "sunshine_lolipops":
        if round >= 3:
            
            if players_last_move and bots_last_move == "r":
                bot_move = 'p' 
                if players_last_move and bots_last_move == "p":
                    bot_move = 's'
                if players_last_move and bots_last_move == "s":
                    bot_move = 'r'
            if num == 10:
                strategy = "EMT"
            if players_last_move == "r":
                if num <= 7:
                    bot_move = "p"
                if num > 7:
                    bot_move = "s"
            if players_last_move == "p":
                    if num <= 8:
                        bot_move = "s"
                    if num >= 9:
                        bot_move = "r"
            if players_last_move == "s":
                if num <= 7:
                    bot_move = "r"
                if num > 7:
                    bot_move = random.choice(["r", "s",])
            if players_last_move== "r" and bots_last_move == "r":
                bot_move = 'p' 
            if players_last_move == "p" and bots_last_move == "p":
                bot_move = 's'
            if players_last_move=="s" and bots_last_move == "s":
                bot_move = 'r'
            
    if strategy == "EMT":
            if round >= 3:
                if players_last_move == "r":
                    if num <= 7:
                        bot_move = "p"
                    if num > 7:
                        bot_move = "r"
                if players_last_move == "p":
                    if num <= 7:
                        bot_move = "s"
                    if num > 7:
                        bot_move = "p"
                if players_last_move == "s":
                    if num <= 7:
                        bot_move = "r"
                    if num > 7:
                        bot_move = "s"
                if num == 10:
                    strategy = "sunshine_lolipops"
                if players_last_move == "r" and bots_last_move == "r":
                    bot_move = 'r'
                if players_last_move == "p" and bots_last_move == "p":
                    bot_move = 'p'
                if players_last_move == "s" and bots_last_move == "s":
                    bot_move = 's'
    if players_last_move == "p" and bots_last_move == "r":
        bot_move = "s"

    if round == 1:
        if num <= 8:
            bot_move = "p"
        if num > 8:
            bot_move = "r"
    if round == 2:
        if bots_last_move == players_last_move:
            if num <= 7:
                bot_move = "r"
            if num > 7:
                bot_move = "s"
        if bots_last_move != players_last_move:
            if num > 7:
                bot_move = "r"
            if num <= 7:
                bot_move = "p"

    if players_move == bot_move:
        result = "wowwie a tie"
        ties += 1
    elif (players_move == "r" and bot_move == "s") or (players_move == "p" and bot_move == "r") or (players_move == "s" and bot_move == "p"):
        result = "ROUND WON!"
        wins += 1
    else:
        result = "ROUND LOST!"
        losses += 1

    print(result)
    print(players_move, "vs", bot_move)
    show_lcd(result, f"{players_move} vs {bot_move}")
    time.sleep(2)

    if round == 10:
        Continue = Continue = input("Keep playing? Y or N: ").strip()
        show_lcd("Keep playing?", Continue)
        
        if Continue == "N":
            break
        if Continue == "Y":
            round = 0
        if Continue == "n":
                break
        if Continue == "y":
                round = 0
        while Continue != "n" or "N" or "Y" or "y":
            show_lcd("try again")
            Continue = show_lcd(input("Y or N: "))
    players_last_move = players_move
    bots_last_move = bot_move

print("Thanks for playing!")
print(wins, "wins", ties, "ties", losses, "losses")
show_lcd(f"W:{wins} T:{ties} L:{losses}")
