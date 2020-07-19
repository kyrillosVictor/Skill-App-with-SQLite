# Skill-App-with-SQLite
Skill app let's you create a database that contains users and their skills
Kyrillos Victor, I'm becoming a pro Python developver and this is a practical app from the lectures of Elzero Academy.

import sqlite3

db = sqlite3.connect("Skills_App.db")

cr = db.cursor()

cr.execute("create table if not exists Skills(User_ID integer, 'Skill' text, Progress integer)")

cr.execute("create table if not exists Users(User_ID integer, 'Name' text)")


def save_and_close():
    """ Commit changes and close connection to Database"""
    db.commit()

    db.close()
    print("Changes are saved, and connection to Database is closed!")


# Intro msg
input_message = """
What do you want to do (kindly type your choice from below!)?\n
"add" => Add a new Username and ID
"users" => Show all users in Database\n
"s" => Show user skills
"a" => Add a new skill
"d" => delete a skill
"u" => Update skill progress
"q" => Quit the app\n
Your Option = """

# Type the option from intro msg
user_input = input(input_message).strip().lower()

# options list
cmd_list = ["add", "users", "s", "a", "d", "u", "q"]

# Set-up methods on CMD list
# All methods should inculde save_and_close() method to guarantee that all changes are saved and connection is closed after saving

def add_user():
    """add a new username and ID"""

    user_name = input("Please type in your name: ").strip().title()
    user_ID = input("Please type in your ID # ").strip()
    cr.execute(f"insert into Users(User_ID, Name) values({user_ID}, '{user_name}')")

    print("A new username and ID have been added")

    save_and_close()


def show_users():

    cr.execute("Select * from Users")
    print(cr.fetchall())

    save_and_close()



def show_skills():

    enter_ID = input("Please type in your ID # ").strip()

    cr.execute(f"select * from Skills where User_ID = {enter_ID}")
    
    skills_list = cr.fetchall()

    cr.execute(f"Select Name from Users where User_ID = {enter_ID}")

    userName = cr.fetchone()

    print(f"Hello {userName}")

    print(f"You have {len(skills_list)} skill/s:")

    for row in skills_list:

        print(f"Skill => {row[1]}, That's progressed to {row[2]}%")

    save_and_close()


def add_skills():
    """add a skill, user_ID and progress"""

    enter_ID = input("Please type in your ID # ").strip()

    cr.execute(f"select Name from Users where User_ID = {enter_ID}")

    userName = cr.fetchone()

    print(f"Hello {userName}")
    
    skill_name = input("Please type the skill name you want to add: ").strip().title()

    cr.execute(f"select Skill from Skills where Skill = '{skill_name}'")

    result = cr.fetchone()

    if result == None:
        
        progress = input("And this skill has progressed to: ").strip()

        cr.execute(f"insert into Skills(User_ID, Skill, Progress) values('{enter_ID}', '{skill_name}', {progress})")

        print("Your skill is added successfully!")

    else:

        print("This skill is already exists!")
        update_choice = input(f"Do you want to update {skill_name}'s progress? [Y/N] ").strip().lower()

        if update_choice == "y":

            new_progress = input("Please type in the updated progress # ").strip()

            cr.execute(f"update Skills set Progress = {new_progress} where Skill = '{skill_name}' and User_ID = {enter_ID}")

            print(f"{skill_name}'s progress has been updated to {new_progress}%")


    save_and_close()


def delete_skills():

    enter_ID = input("Please type in your ID # ").strip()

    cr.execute(f"Select Name from Users where User_ID = {enter_ID}")

    userName = cr.fetchone()

    print(f"Hello {userName}")
    
    skill_name = input("Type in the skill name you want to delete: ").strip().title()

    cr.execute(f"delete from Skills where Skill = '{skill_name}' and User_ID = {enter_ID}")

    print(f"{skill_name} has been deleted!")


    save_and_close()


def update_skills():
    """ showing the skills so the user can type the skill correctly that they want to update
    update skill progress referred by user_ID and"""

    enter_ID = input("Please type in your ID # ").strip()

    cr.execute(f"select * from Skills where User_ID = {enter_ID}")
    
    skills_list = cr.fetchall()

    cr.execute(f"select Name from Users where User_ID = {enter_ID}")

    userName = cr.fetchone()

    print(f"Hello {userName}")

    print(f"You have {len(skills_list)} skill/s:")

    for row in skills_list:

        print(f"Skills => {row[1]}, That's progressed to {row[2]}%")

    skill = input("Please type in the skill you want to update it's progress: ").strip().title()

    new_progress = input("Please type in the updated progress # ").strip()

    cr.execute(f"update Skills set Progress = {new_progress} where Skill = '{skill}' and User_ID = {enter_ID}")

    print(f"{skill}'s progress has been updated to {new_progress}%")


    save_and_close()


def quit_skills():
    print("Quit Skills")
    save_and_close()



if user_input in cmd_list:

    if user_input == "s":

        show_skills()


    elif user_input == "a":

        add_skills()


    elif user_input == "d":

        delete_skills()


    elif user_input == "u":

        update_skills()


    elif user_input == "add":

        add_user()


    elif user_input == "users":

        show_users()


    else:

        save_and_close()

else:

    print(f"Your input \"{user_input}\" is incorrect")

    save_and_close()
