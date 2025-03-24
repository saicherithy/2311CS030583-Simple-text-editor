 2311CS030583-Simple-text-editor
Created a simple text editor in Python using arrays, stacks, and queues. It supports text insertion, deletion, undo/redo, find and replace, and a print queue, with file handling for saving and loading data.
import os

class TextEditor:
    def __init__(self):
        self.text = []
        self.undo_stack = []
        self.redo_stack = []
        self.print_queue = []

    def insert_text(self, text):
        self.text.append(text + "\n")
        self.undo_stack.append(text + "\n")
        print("\n✅ Text inserted successfully!")
        self.print_text()

    def delete_text(self):
        if self.text:
            deleted_text = self.text.pop()
            self.undo_stack.append(deleted_text)
            print(f"\n✅ Deleted: {deleted_text.strip()}")
        else:
            print("\n⚠️ No text to delete.")
        self.print_text()

    def undo(self):
        if self.undo_stack:
            undone_text = self.undo_stack.pop()
            self.redo_stack.append(undone_text)
            self.text.append(undone_text)
            print("\n🔄 Undo performed!")
        else:
            print("\n⚠️ Nothing to undo.")
        self.print_text()

    def redo(self):
        if self.redo_stack:
            redone_text = self.redo_stack.pop()
            self.undo_stack.append(redone_text)
            self.text.append(redone_text)
            print("\n🔄 Redo performed!")
        else:
            print("\n⚠️ Nothing to redo.")
        self.print_text()

    def find_text(self, text):
        for i, line in enumerate(self.text):
            if text in line:
                print(f"\n🔍 Text found at line {i + 1}: {line.strip()}")
                return
        print("\n❌ Text not found.")

    def replace_text(self, old_text, new_text):
        replaced = False
        for i, line in enumerate(self.text):
            if old_text in line:
                self.text[i] = line.replace(old_text, new_text)
                replaced = True
        if replaced:
            print(f"\n✅ Replaced '{old_text}' with '{new_text}'!")
        else:
            print(f"\n❌ '{old_text}' not found.")
        self.print_text()

    def print_text(self):
        print("\n📄 Current Text:")
        if self.text:
            print("".join(self.text))
        else:
            print("(No text available)")

    def add_print_job(self, text):
        self.print_queue.append(text)
        print(f"\n🖨 Print job '{text}' added!")

    def delete_specific_print_job(self):
        if not self.print_queue:
            print("\n⚠️ No print jobs to delete.")
            return
        
        self.view_print_jobs()
        try:
            index = int(input("\nEnter the print job number to delete: ")) - 1
            if 0 <= index < len(self.print_queue):
                removed_job = self.print_queue.pop(index)
                print(f"\n✅ Print job '{removed_job}' deleted successfully!")
            else:
                print("\n❌ Invalid job number. Please try again.")
        except ValueError:
            print("\n❌ Invalid input! Please enter a number.")

    def view_print_jobs(self):
        print("\n🖨 Print Queue:")
        if self.print_queue:
            for i, job in enumerate(self.print_queue, 1):
                print(f"{i}. {job}")
        else:
            print("(No print jobs available)")

def main():
    editor = TextEditor()

    while True:
        print("\n📌 Text Editor Menu:")
        print("1. Insert Text")
        print("2. Delete Text")
        print("3. Undo")
        print("4. Redo")
        print("5. Find Text")
        print("6. Replace Text")
        print("7. Print Text")
        print("8. Add Print Job")
        print("9. Delete Specific Print Job")
        print("12. View Print Jobs")

        choice = input("\nEnter your choice: ")

        if choice == "1":
            text = input("Enter text to insert: ")
            editor.insert_text(text)
        elif choice == "2":
            editor.delete_text()
        elif choice == "3":
            editor.undo()
        elif choice == "4":
            editor.redo()
        elif choice == "5":
            text = input("Enter text to find: ")
            editor.find_text(text)
        elif choice == "6":
            old_text = input("Enter text to replace: ")
            new_text = input("Enter new text: ")
            editor.replace_text(old_text, new_text)
        elif choice == "7":
            editor.print_text()
        elif choice == "8":
            text = input("Enter print job name: ")
            editor.add_print_job(text)
        elif choice == "9":
            editor.delete_specific_print_job()
        elif choice == "12":
            editor.view_print_jobs()
        else:
            print("\n❌ Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
