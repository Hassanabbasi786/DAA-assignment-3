# DAA-assignment-3
#include <iostream>

using namespace std;

const int SIZE = 1000;

struct OperationCount {
    long comparisons = 0;
    long swaps = 0;
};

// Bubble Sort
void bubbleSort(int arr[], int n, OperationCount& ops) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            ops.comparisons++;
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                ops.swaps++;
            }
        }
    }
}


void selectionSort(int arr[], int n, OperationCount& ops) {
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            ops.comparisons++;
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            int temp = arr[minIdx];
            arr[minIdx] = arr[i];
            arr[i] = temp;
            ops.swaps++;
        }
    }
}

void merge(int arr[], int left, int mid, int right, OperationCount& ops) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    int L[n1], R[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int i = 0; i < n2; i++) R[i] = arr[mid + 1 + i];

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        ops.comparisons++;
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
        ops.swaps++;
    }
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(int arr[], int left, int right, OperationCount& ops) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid, ops);
    mergeSort(arr, mid + 1, right, ops);
    merge(arr, left, mid, right, ops);
}

// Quick Sort helper function
int partition(int arr[], int low, int high, OperationCount& ops) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j <= high - 1; j++) {
        ops.comparisons++;
        if (arr[j] < pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            ops.swaps++;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    ops.swaps++;
    return i + 1;
}

void quickSort(int arr[], int low, int high, OperationCount& ops) {
    if (low < high) {
        int pi = partition(arr, low, high, ops);
        quickSort(arr, low, pi - 1, ops);
        quickSort(arr, pi + 1, high, ops);
    }
}

void runAndCountOperations(void (*sortFunction)(int[], int, OperationCount&), int arr[], int n, const string& sortName, const string& caseType) {
    int tempArr[SIZE];
    for (int i = 0; i < n; i++) tempArr[i] = arr[i];
    
    OperationCount ops;
    sortFunction(tempArr, n, ops);
    cout << sortName << " on " << caseType << " array - Comparisons: " << ops.comparisons << ", Swaps: " << ops.swaps << endl;
}

void runAndCountOperations(void (*sortFunction)(int[], int, int, OperationCount&), int arr[], int n, const string& sortName, const string& caseType) {
    int tempArr[SIZE];
    for (int i = 0; i < n; i++) tempArr[i] = arr[i];
    
    OperationCount ops;
    sortFunction(tempArr, 0, n - 1, ops);
    cout << sortName << " on " << caseType << " array - Comparisons: " << ops.comparisons << ", Swaps: " << ops.swaps << endl;
}

int main() {
    int bestCase[SIZE], averageCase[SIZE], worstCase[SIZE];

    // Initialize arrays for best, average, and worst cases
    for (int i = 0; i < SIZE; i++) {
        bestCase[i] = i;                   // Sorted array (Best Case)
        averageCase[i] = rand() % SIZE;    // Random array (Average Case)
        worstCase[i] = SIZE - i;           // Reverse sorted array (Worst Case)
    }

    cout << "Bubble Sort Results:\n";
    runAndCountOperations(bubbleSort, bestCase, SIZE, "Bubble Sort", "Best Case");
    runAndCountOperations(bubbleSort, averageCase, SIZE, "Bubble Sort", "Average Case");
    runAndCountOperations(bubbleSort, worstCase, SIZE, "Bubble Sort", "Worst Case");

    cout << "\nSelection Sort Results:\n";
    runAndCountOperations(selectionSort, bestCase, SIZE, "Selection Sort", "Best Case");
    runAndCountOperations(selectionSort, averageCase, SIZE, "Selection Sort", "Average Case");
    runAndCountOperations(selectionSort, worstCase, SIZE, "Selection Sort", "Worst Case");

    cout << "\nMerge Sort Results:\n";
    runAndCountOperations(mergeSort, bestCase, SIZE, "Merge Sort", "Best Case");
    runAndCountOperations(mergeSort, averageCase, SIZE, "Merge Sort", "Average Case");
    runAndCountOperations(mergeSort, worstCase, SIZE, "Merge Sort", "Worst Case");

    cout << "\nQuick Sort Results:\n";
    runAndCountOperations(quickSort, bestCase, SIZE, "Quick Sort", "Best Case");
    runAndCountOperations(quickSort, averageCase, SIZE, "Quick Sort", "Average Case");
    runAndCountOperations(quickSort, worstCase, SIZE, "Quick Sort", "Worst Case");

    return 0;
}
