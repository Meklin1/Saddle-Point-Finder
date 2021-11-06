/// Meinardas Klinkovas, ex. 2
/// Program finds saddle point coordinates from the table.
#include <stdio.h>
#include <conio.h>
#include <string.h>
#include <stdlib.h>
#define MAX_N 9999
#define MIN_N 2
/// function checks if program does not exceed memory limit with input value of N
int input_N (int ***data, int **numbers);
/// function puts saddle point coordinates to an array and counts their number.
void find_saddle_points (int **data, int saddle_points[4][2], int N, int* saddle_point_count);
/// function scans entered number.
int scan_and_validate (int min, int max);
/// function puts numbers to an array from input.
void put_to_array (int **data, int N, int *numbers);
/// function prints saddle point coordinates and the array values with these coordinates
void print_saddle_points (int **data, int N, int count, int points[4][2]);
/// function repeatedly prints the matrix with input values
void print_table (int **data, int N, int len);
/// function allocates memory to an array
int** give_memory (int N);
/// function deallocates memory from an array
void free_memory (int **data, int N);

int main ()
{
    printf("The program finds the coordinates of the saddle points from a table filled with user-entered numbers.\n");
	int saddle_point_count = 0;
    int saddle_points[4][2]; /// array of coordinates of saddle points, there are maximum 4 saddle points
	int **data; /// array of input numbers
	int *numbers; /// array for storing non-repetitive numbers
    int N = input_N(&data, &numbers);
	put_to_array(data, N, numbers);
    find_saddle_points(data, saddle_points, N, &saddle_point_count);
    print_saddle_points(data, N, saddle_point_count, saddle_points);
    free_memory(data, N);

	return 0;
}
int input_N(int ***data, int **numbers)
{
    int N;
    printf("Enter a number of rows and columns from range [%d; %d].\n", MIN_N, MAX_N);
    ///repetitively scanning N value if memory boundaries are exceeded
    do{
        free(*numbers);
        N = scan_and_validate(MIN_N,MAX_N);
        *data = give_memory(N);
        *numbers = (int*)calloc(N * N, sizeof(int));
        if(*data == NULL || *numbers == NULL){
            printf("Memory exceeded. Enter a number of rows and columns again.\n");
        }
    }
    while(*data == NULL || *numbers == NULL);
    printf("Size of rows and columns is entered successfully.\n");
    return N;
}

int** give_memory(int N)
{

    int  **data = (int** )calloc(N, sizeof(int*));
    if(!data){
        return NULL;
    }
    ///allocating memory to pointer, if memory limit is exceeded, NULL is returned and already used memory is dellocated
    for (int i = 0; i < N; ++i)
    {
        data[i] = (int* )calloc(N, sizeof(int));
        if(!data[i]){
            for(int j = 0; j < i; ++j){
                free(data[j]);
            }
            free(data);

            return NULL;
        }
    }
    return data;
}
void find_saddle_points(int **data, int saddle_points[4][2], int N, int* saddle_point_count)
{

    for (int i = 0; i < N; i++)
    {
        if(*saddle_point_count == 4)
            break;
        int min_row = data[i][0], min_col_index = 0;
        int max_row = data[i][0], max_col_index = 0;
        int saddle_point1 = 1, saddle_point2 = 1;
        ///finding minimum and maximum row values and and column indexes of these values
        for (int j = 1; j < N; ++j)
        {
            if (min_row > data[i][j])
            {
                min_row = data[i][j];
                min_col_index = j;
            }
            if (max_row < data[i][j])
            {
                max_row = data[i][j];
                max_col_index = j;
            }
        }
        ///searching if there are any higher and lower values in a row
        for(int k = 0; k < N; ++k)
        {
            if(data[k][min_col_index] >  min_row)
                saddle_point1 = 0;
            if(data[k][max_col_index] <  max_row)
                saddle_point2 = 0;

        }
        if(saddle_point1)
        {
            saddle_points[*saddle_point_count][0] = i;
            saddle_points[*saddle_point_count][1] = min_col_index;
            ++*saddle_point_count;
        }
        if(saddle_point2)
        {
            saddle_points[*saddle_point_count][0] = i;
            saddle_points[*saddle_point_count][1] = max_col_index;
            ++*saddle_point_count;
        }
    }
}
int scan_and_validate(int min, int max)
{
    long long number;
    ///scanning input value while it is not an integer
    while(1)
    {
        char line[99];
        int count; /// length of string
        scanf("%98s",line);
        if(sscanf(line,"%10lld%n", &number, &count)){
                if(count == strlen(line)){
                    if(number >= min && number <= max)
                        break;
                }

           }
        printf("Wrong input, you can only enter integers. Try again.\n");

    }
    return number;
}
void put_to_array(int **data, int N, int *numbers)
{
    int temp; /// temporary number for counting length of a number
    int int_len; /// temporary length of current integer
    int len = 0; /// needed space for table alignment
    int flag2 = 1; /// flag for printing message to user
    for(int i = 0; i < N; ++i)
        for(int j = 0; j < N; ++j){
            int flag1 = 1; /// flag for recognising if input does not repeat
            if(flag2){
                printf("%d more  numbers need to be input.\n",N * N - (j + i * N));
                printf("Enter non-repetitive integer from range [%d; %d] to fill the table.\n", INT_MIN, INT_MAX);
            }
            * (* (data + i) + j) = scan_and_validate(INT_MIN, INT_MAX);
            for(int k = 0; k <= j + N * i; ++k)
            {
                ///putting input value to an array if it does not repeat with before entered numbers
                if(j + N * i  == k){
                    flag2 = 1;
                    numbers[k] = * (* (data + i) + j);
                    break;
                }
                ///checking if input number is repeating
                if(* (* (data + i) + j) == numbers[k]){
                    --j;

                    printf("Enter non-repetitive number.\n");
                    flag1 = 0;
                    flag2 = 0;
                    break;
                }
            }
            if(flag1){
                temp = * (* (data + i) + j);
                if(temp >= 0){
                    int_len = 0;
                }
                else{
                    int_len = 1;
                }

                do{
                    temp /= 10;
                    ++int_len;
                }
                while(temp != 0);

                if(int_len > len){
                    len = int_len;
                }
                print_table(data, N, len);
                printf("Input is correct.\n");
            }
        }

}

void print_saddle_points(int **data, int N, int count, int points[4][2])
{
    if(count > 0){
        printf("Coordinates of saddle points:\n");
        for(int i = 0; i < count; ++i)
        printf("(%d; %d)\n", points[i][0] + 1, points[i][1] + 1);
    }
    else{
        printf("No saddle points were found.\n");
    }

}
void print_table(int **data, int N, int len)
{
    ///function clears terminal
    system("cls");
    for(int i = 0; i < N; ++i){
        for(int j = 0; j < N; ++j){
            printf("|%*d", len,* (* (data + i) + j));
        }
        printf("|\n");
    }
}
void free_memory(int **data, int N)
{
    for(int i = 0; i < N; i++)
        free(data[i]);
    free(data);
}
