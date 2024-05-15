#include <iostream>
#include <fstream>
#include <string>

using namespace std;

// Структура, що представляє завдання
struct Task {
    string title;         // Заголовок завдання
    string description;   // Опис завдання
    bool completed;       // Прапорець, що позначає, чи завершено завдання

    // Конструктор структури Task
    Task(string t, string d, bool c) : title(t), description(d), completed(c) {}
};

// Функція для додавання завдання
void addTask(Task* tasks[], int& taskCount) {
    string title, description;
    cout << "Введiть заголовок завдання: ";
    cin.ignore();
    getline(cin, title);
    cout << "Введiть опис завдання: ";
    getline(cin, description);
    tasks[taskCount++] = new Task(title, description, false);
    cout << "Завдання успiшно додано.\n";
}

// Функція для позначення завдання як завершене
void markTaskCompleted(Task* tasks[], int taskCount) {
    int index;
    cout << "Введiть iндекс завдання, яке потрiбно позначити як завершене: ";
    cin >> index;
    if (index >= 0 && index < taskCount) {
        tasks[index]->completed = true;
        cout << "Завдання позначено як завершене.\n";
    }
    else {
        cout << "Недiйсний iндекс завдання.\n";
    }
}

// Функція для відображення всіх завдань
void displayTasks(Task* tasks[], int taskCount) {
    if (taskCount == 0) {
        cout << "Список завдань порожнiй.\n";
    }
    else {
        cout << "Список завдань:\n";
        for (int i = 0; i < taskCount; ++i) {
            cout << i << ". " << tasks[i]->title << " - " << tasks[i]->description;
            if (tasks[i]->completed) {
                cout << " (Завершено)";
            }
            cout << endl;
        }
    }
}

// Функція для збереження завдань у текстовий файл
void saveTasksToFile(Task* tasks[], int taskCount, const string& filename) {
    ofstream outputFile(filename);
    if (outputFile.is_open()) {
        for (int i = 0; i < taskCount; ++i) {
            outputFile << tasks[i]->title << endl;
            outputFile << tasks[i]->description << endl;
            outputFile << tasks[i]->completed << endl;
        }
        outputFile.close();
        cout << "Завдання збережено у файлi " << filename << endl;
    }
    else {
        cout << "Не вдалося вiдкрити файл для запису." << endl;
    }
}

int main() {
    setlocale(LC_ALL, "Ukrainian");
    const int MAX_TASKS = 100; // Максимальна кількість завдань
    Task* tasks[MAX_TASKS];
    int taskCount = 0;
    int choice;

    do {
        cout << "Менеджер завдань\n";
        cout << "1. Додати завдання\n";
        cout << "2. Позначити завдання як завершене\n";
        cout << "3. Показати всi завдання\n";
        cout << "4. Зберегти завдання у файл\n";
        cout << "5. Вихiд\n";
        cout << "Виберiть опцiю: ";
        cin >> choice;

        switch (choice) {
        case 1:
            if (taskCount < MAX_TASKS) {
                addTask(tasks, taskCount);
            }
            else {
                cout << "Досягнуто максимальної кiлькостi завдань.\n";
            }
            break;
        case 2:
            markTaskCompleted(tasks, taskCount);
            break;
        case 3:
            displayTasks(tasks, taskCount);
            break;
        case 4: {
            string filename;
            cout << "Введiть назву файлу: ";
            cin >> filename;
            saveTasksToFile(tasks, taskCount, filename);
            break;
        }
        case 5:
            cout << "До побачення!\n";
            break;
        default:
            cout << "Недiйсний вибiр. Будь ласка, спробуйте знову.\n";
            break;
        }

    } while (choice != 5);

    // Звільнення пам'яті, виділеної для динамічних завдань
    for (int i = 0; i < taskCount; ++i) {
        delete tasks[i];
    }

    return 0;
}
