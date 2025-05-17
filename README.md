#include <iostream>
#include <cstdlib>
#include <ctime>

using namespace std;

const int MAX_ROW = 5;
const int MAX_COL = 5;

// Function declarations
double sumOfRow(const double mat[][MAX_COL], int row, int maxRow);
double sumOfCol(const double mat[][MAX_COL], int column, int maxRow); // Has a known bug
void fillWithRandomNum(double mat[][MAX_COL], int maxRow);
void printMatrix(const double mat[][MAX_COL], int maxRow);
double findMax(const double mat[][MAX_COL], int maxRow);
double findMin(const double mat[][MAX_COL], int maxRow);
double average(const double mat[][MAX_COL], int maxRow);
double* sumOfRows(const double mat[][MAX_COL], const int maxRow);
double* sumOfCols(const double mat[][MAX_COL], const int maxRow); // ✅ New function

// Function definitions
double sumOfRow(const double mat[][MAX_COL], int row, int maxRow) {
    double sum = 0.0;
    for (int j = 0; j < MAX_COL; ++j)
        sum += mat[row][j];
    return sum;
}

double sumOfCol(const double mat[][MAX_COL], int column, int maxRow) {
    double sum = 0.0;
    // ❌ BUG: skips the last row
    for (int i = 0; i < maxRow - 1; ++i)
        sum += mat[i][column];
    return sum;
}

void fillWithRandomNum(double mat[][MAX_COL], int maxRow) {
    srand(static_cast<unsigned int>(time(0)));
    for (int i = 0; i < maxRow; ++i)
        for (int j = 0; j < MAX_COL; ++j)
            mat[i][j] = (rand() % 1000) / 10.0;
}

void printMatrix(const double mat[][MAX_COL], int maxRow) {
    for (int i = 0; i < maxRow; ++i) {
        for (int j = 0; j < MAX_COL; ++j)
            cout << mat[i][j] << "\t";
        cout << "\n";
    }
}

double findMax(const double mat[][MAX_COL], int maxRow) {
    double maxVal = mat[0][0];
    for (int i = 0; i < maxRow; ++i)
        for (int j = 0; j < MAX_COL; ++j)
            if (mat[i][j] > maxVal)
                maxVal = mat[i][j];
    return maxVal;
}

double findMin(const double mat[][MAX_COL], int maxRow) {
    double minVal = mat[0][0];
    for (int i = 0; i < maxRow; ++i)
        for (int j = 0; j < MAX_COL; ++j)
            if (mat[i][j] < minVal)
                minVal = mat[i][j];
    return minVal;
}

double average(const double mat[][MAX_COL], int maxRow) {
    double total = 0.0;
    int count = 0;
    for (int i = 0; i < maxRow; ++i)
        for (int j = 0; j < MAX_COL; ++j) {
            total += mat[i][j];
            ++count;
        }
    return total / count;
}

double* sumOfRows(const double mat[][MAX_COL], const int maxRow) {
    double* rowSums = new double[maxRow];
    for (int i = 0; i < maxRow; ++i) {
        rowSums[i] = 0.0;
        for (int j = 0; j < MAX_COL; ++j)
            rowSums[i] += mat[i][j];
    }
    return rowSums;
}

double* sumOfCols(const double mat[][MAX_COL], const int maxRow) {
    double* colSums = new double[MAX_COL];
    for (int j = 0; j < MAX_COL; ++j) {
        colSums[j] = 0.0;
        for (int i = 0; i < maxRow; ++i)
            colSums[j] += mat[i][j];
    }
    return colSums;
}

// Main program with menu
int main() {
    double matrix[MAX_ROW][MAX_COL];
    int choice, index;

    fillWithRandomNum(matrix, MAX_ROW);

    do {
        cout << "\nMatrix Menu:\n";
        cout << "1. Print Matrix\n";
        cout << "2. Sum of Row\n";
        cout << "3. Sum of Column (has a bug!)\n";
        cout << "4. Find Maximum\n";
        cout << "5. Find Minimum\n";
        cout << "6. Average of All Elements\n";
        cout << "7. Sum of All Rows (dynamic)\n";
        cout << "8. Sum of All Columns (dynamic)\n";
        cout << "0. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            printMatrix(matrix, MAX_ROW);
            break;
        case 2:
            cout << "Enter row index (0 to " << MAX_ROW - 1 << "): ";
            cin >> index;
            if (index >= 0 && index < MAX_ROW)
                cout << "Sum of row " << index << " = " << sumOfRow(matrix, index, MAX_ROW) << "\n";
            else
                cout << "Invalid row index.\n";
            break;
        case 3:
            cout << "Enter column index (0 to " << MAX_COL - 1 << "): ";
            cin >> index;
            if (index >= 0 && index < MAX_COL)
                cout << "Sum of column " << index << " = " << sumOfCol(matrix, index, MAX_ROW) << "\n";
            else
                cout << "Invalid column index.\n";
            break;
        case 4:
            cout << "Maximum value = " << findMax(matrix, MAX_ROW) << "\n";
            break;
        case 5:
            cout << "Minimum value = " << findMin(matrix, MAX_ROW) << "\n";
            break;
        case 6:
            cout << "Average = " << average(matrix, MAX_ROW) << "\n";
            break;
        case 7: {
            double* rowSums = sumOfRows(matrix, MAX_ROW);
            for (int i = 0; i < MAX_ROW; ++i)
                cout << "Sum of row " << i << " = " << rowSums[i] << "\n";
            delete[] rowSums;
            break;
        }
        case 8: {
            double* colSums = sumOfCols(matrix, MAX_ROW);
            for (int j = 0; j < MAX_COL; ++j)
                cout << "Sum of column " << j << " = " << colSums[j] << "\n";
            delete[] colSums;
            break;
        }
        case 0:
            cout << "Exiting...\n";
            break;
        default:
            cout << "Invalid choice. Please try again.\n";
        }

    } while (choice != 0);

    return 0;
}


// New Logical Bug
// sumOfCol skips the last row due to i < maxRow - 1 instead of i < maxRow.
