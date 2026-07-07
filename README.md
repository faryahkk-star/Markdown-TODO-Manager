from pathlib import Path


class TodoManager:
    def __init__(self, filename="todo.md"):
        self.file = Path(filename)

        if not self.file.exists():
            self.file.write_text("# TODO List\n\n", encoding="utf-8")

    def load(self):
        lines = self.file.read_text(encoding="utf-88").splitlines()

        tasks = []

        for line in lines:
            if line.startswith("- [ ]") or line.startswith("- [x]"):
                tasks.append(line)

        return tasks

    def save(self, tasks):
        content = "# TODO List\n\n"

        for task in tasks:
            content += task + "\n"

        self.file.write_text(content, encoding="utf-8")

    def add(self, task):
        tasks = self.load()
        tasks.append(f"- [ ] {task}")
        self.save(tasks)
        print("Task added.")

    def list(self):
        tasks = self.load()

        if not tasks:
            print("No tasks.")
            return

        print()

        for i, task in enumerate(tasks, 1):
            print(f"{i}. {task}")

    def complete(self, index):
        tasks = self.load()

        if 0 <= index < len(tasks):
            tasks[index] = tasks[index].replace("- [ ]", "- [x]", 1)
            self.save(tasks)
            print("Task completed.")
        else:
            print("Invalid task.")

    def remove(self, index):
        tasks = self.load()

        if 0 <= index < len(tasks):
            removed = tasks.pop(index)
            self.save(tasks)
            print(f"Removed: {removed}")
        else:
            print("Invalid task.")


def main():
    manager = TodoManager()

    while True:
        print("\n==== Markdown TODO Manager ====")
        print("1. List Tasks")
        print("2. Add Task")
        print("3. Complete Task")
        print("4. Remove Task")
        print("5. Exit")

        choice = input("\nSelect: ")

        if choice == "1":
            manager.list()

        elif choice == "2":
            task = input("Task: ")
            manager.add(task)

        elif choice == "3":
            manager.list()
            index = int(input("Task number: ")) - 1
            manager.complete(index)

        elif choice == "4":
            manager.list()
            index = int(input("Task number: ")) - 1
            manager.remove(index)

        elif choice == "5":
            break

        else:
            print("Invalid option.")


if __name__ == "__main__":
    main()
