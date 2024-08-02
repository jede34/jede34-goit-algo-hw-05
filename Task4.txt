def input_error(func):
    def inner(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except IndexError:
            return "Enter the argument for the command."
        except ValueError:
            return "Give me name and phone please."
        except KeyError:
            return "Contact not found."
    return inner

def parse_input(user_input):
    """
    Функція розбиває введений рядок на команду та аргументи.
    Використовується для парсингу введених команд користувача.
    """
    cmd, *args = user_input.split()
    cmd = cmd.strip().lower()
    return cmd, args

@input_error
def add_contact(args, contacts):
    """
    Функція додає новий контакт до словника contacts.
    args має бути списком з ім'ям та номером телефону.
    """
    if len(args) != 2:
        raise ValueError("Give me name and phone please.")

    name, phone = args
    contacts[name] = phone
    return "Contact added."

@input_error
def change_contact(args, contacts):
    """
    Функція змінює існуючий номер телефону для вказаного контакту.
    args має бути списком з ім'ям та новим номером телефону.
    """
    if len(args) != 2:
        raise ValueError("Give me name and phone please.")

    name, new_phone = args
    if name in contacts:
        contacts[name] = new_phone
        return "Contact updated."
    else:
        raise KeyError(f"Contact '{name}' not found.")

@input_error
def show_phone(args, contacts):
    """
    Функція виводить номер телефону для вказаного контакту.
    args має бути списком з ім'ям контакту.
    """
    if len(args) != 1:
        raise ValueError("Enter user name.")

    name = args[0]
    if name in contacts:
        return contacts[name]
    else:
        raise KeyError(f"Contact '{name}' not found.")

@input_error
def show_all(contacts):
    """
    Функція виводить усі збережені контакти разом з номерами телефонів.
    """
    if not contacts:
        return "No contacts found."

    all_contacts = "\n".join(f"{name}: {contacts[name]}" for name in sorted(contacts))
    return all_contacts

def main():
    contacts = {}
    print("Welcome to the assistant bot!")
    while True:
        user_input = input("Enter a command: ")
        command, args = parse_input(user_input)

        if command in ["close", "exit"]:
            print("Good bye!")
            break

        elif command == "hello":
            print("How can I help you?")

        elif command == "add":
            print(add_contact(args, contacts))

        elif command == "change":
            print(change_contact(args, contacts))

        elif command == "phone":
            print(show_phone(args, contacts))

        elif command == "all":
            print(show_all(contacts))

        else:
            print("Invalid command.")

if __name__ == "__main__":
    main()
