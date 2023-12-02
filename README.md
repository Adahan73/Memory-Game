# Memory-Game
This program defines a simple console-based memory matching game in C++. You can adjust the boardSize variable to change the size of the game board. The game continues until all pairs are matched, and it displays the total number of moves taken throughout the game.

#include <iostream>
#include <vector>
#include <algorithm>
#include <ctime>
#include <cstdlib>
using namespace std;

vector<int> initializeBoard(int size) {
    vector<int> cards;
    for (int i = 1; i <= size / 2; ++i) {
        cards.push_back(i);
        cards.push_back(i);
    }
 
    random_shuffle(cards.begin(), cards.end());
    return cards;
}

void displayBoard(const vector<int>& board, const vector<bool>& revealed) {
    for (int i = 0; i < board.size(); ++i) {
        if (revealed[i]) {
            cout << board[i] << " ";
        } else {
            cout << "X ";
        }

        if ((i + 1) % 4 == 0) {
            cout << endl;
        }
    }
    cout << endl;
}

bool isGameFinished(const vector<bool>& revealed) {
    return all_of(revealed.begin(), revealed.end(), [](bool value) { return value; });
}

int main() {
    srand(static_cast<unsigned int>(time(nullptr)));

    const int boardSize = 8; 
    vector<int> board = initializeBoard(boardSize);
    vector<bool> revealed(boardSize, false);

    int moves = 0;

    while (!isGameFinished(revealed)) {
        cout << "Current Board:" << endl;
        displayBoard(board, revealed);

        int index1, index2;

        do {
            cout << "Enter the indices of two cards to flip (1 to " << boardSize << "): ";
            cin >> index1 >> index2;
        } while (index1 < 1 || index1 > boardSize || index2 < 1 || index2 > boardSize || index1 == index2);

        --index1;
        --index2;

        revealed[index1] = true;
        revealed[index2] = true;

        cout << "Flipped Cards: " << board[index1] << " and " << board[index2] << endl;

        if (board[index1] != board[index2]) {
       
            revealed[index1] = false;
            revealed[index2] = false;
            cout << "No match! Cards flipped back." << endl;
        } else {
            cout << "Match found!" << endl;
        }

        moves++;
    }

    cout << "Congratulations! You completed the game in " << moves << " moves." << endl;

    return 0;
}
