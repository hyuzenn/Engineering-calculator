# 공학용 계산기 프로그램

이 프로젝트는 다양한 수학적 계산을 수행할 수 있는 공학용 계산기를 구현합니다. 사용자는 적분, 로그, 다항식, 삼각함수, 지수 함수, 수열 계산 및 팩토리얼 계산 등을 수행할 수 있습니다.

## 기능

- 적분 계산
- 로그 함수 계산
- 다항 함수 계산
- 삼각 함수 계산
- 지수 함수 계산
- 등차 수열 및 등비 수열 계산
- 팩토리얼 계산
- 이전 계산 값 불러오기

## 코드

아래는 계산기 프로그램의 코드입니다.

```c
#include <stdio.h>
#include <math.h>
#include <string.h>

#define max_memory_size 10
#define func_num 9 // 계산기 기능 개수

int starting (){
    printf("공학용 계산기를 실행합니다. \n\n");
    printf("---계산 기능--- \n");
    printf("1. 적분 계산 \n");
    printf("2. 로그함수 계산 \n");
    printf("3. 다항함수 계산 \n");
    printf("4. 삼각함수 계산 \n");
    printf("5. 지수함수 계산 \n");
    printf("6. 등차수열 계산 \n");
    printf("7. 등비수열 계산 \n");
    printf("8. 팩토리얼 계산 \n");
    printf("9. 이전 계산 값 불러오기 \n");

    int user; // 사용자에게 원하는 기능 입력받기

    do {
        printf("원하는 계산 기능을 입력하세요. ");
        scanf("%d", &user);

    } while (user < 0 || user > func_num); // 1부터 func_num 사이의 값을 받을 때까지 반복 
    printf("\n");
    return user; 
}

int closing (){
    int choice;
    printf("\n1. 다시 시작하기\n");
    printf("2. 종료하기\n");
    printf("원하는 작업을 선택하세요: ");
    scanf("%d", &choice);
    return choice;
}

double integrate_cal(double x); //적분 계산 함수 선언
double coefficient[6]; //적분할 다항식의 계수
double log_cal(int log_type, int antilog); // 로그함수 계산 함수 선언
double polynomial_cal(double x1, double* coefficients, int degree); // 다항함수 계산 함수 선언				
double trigon_cal(int trigon_type, int trigon_type2, double radian); // 삼각함수 계산 함수 선언
double exponential_cal(int x); // 지수함수 계산 선언
double arithmetic_seq_cal(int a, int d, int n); // 등차수열 계산 함수 선언
double geometric_seq_cal(int a, int r, int n); // 등비수열 계산 함수 선언
int fact_cal(int num); // 팩토리얼 계산 함수 선언
void ans_memory(double value, double calculations[], int *count);

int main(){
    double calculations[max_memory_size] = {}; // 계산값을 저장할 배열
    double value = 0;
    int count = 0;
    int user, choice;
    
    //적분
    int i; 
    double u, v, S=0, dx=0;
    //로그함수
    int log_type, antilog;
    //다항함수
    double coefficients[6];
    int degree;
    double x1; 
    //삼각함수
    int trigon_type, trigon_type2;
    double radian;
    // 지수함수
    int x;
    //등차등비
    int a, d, r, n;
    //팩토리얼
    int fact_num;
    
    do {
        user = starting();
        switch(user) {
            case 1:
                printf("---적분 계산--- \n");
                printf("적분할 다항식의 계수를 차수에 맞게 입력하세요.\n");
                
                for (i = 0; i < 6; i++){
                    printf("%d차 항의 계수를 입력하세요. \n", i);
                    scanf("%lf", &coefficient[i]);//적분할 다항식의 계수를 입력받음
                }
                printf("적분 구간이 시작되는 수를 입력하세요. \n");
                scanf("%lf", &u);
                printf("적분 구간이 종료되는 수를 입력하세요. \n");
                scanf("%lf", &v); //적분 구간을 입력받음
                dx = (v - u)/50000; //n=50000일 때 소수점 세자리까지 정확함
                
                for(i=1; i<=50000; i++){
                    value += integrate_cal(u + dx * i) * dx; // 리만 합 계산
                }

                ans_memory(value, calculations, &count);
                break;
            case 2:
                printf("---로그 계산--- \n");
                printf("로그 종류 \n");
                printf("1. 자연로그 \n");
                printf("2. 상용로그 \n");
                
                printf("로그 종류를 선택하시오. ");
                scanf("%d", &log_type);
                printf("진수를 정수의 형태로 입력하시오. ");
                scanf("%d", &antilog);
                
                value = log_cal(log_type, antilog);
                ans_memory(value, calculations, &count);
                break;
            case 3:
                printf("---다항함수 계산--- \n");
                printf("다항식의 최고 차수를 입력하세요: ");
                scanf("%d", &degree);

                printf("각 항의 계수를 입력하세요:\n");
                for (int i = 0; i <= degree; i++) {
                    printf("x^%d의 계수: ", i);
                    scanf("%lf", &coefficients[i]);
                }
                
                printf("다항식의 계수 입력이 완료되었습니다.\n");
                printf("다항식을 계산할 x1 값을 입력하세요: ");
                scanf("%lf", &x1);
                
                value = polynomial_cal(x1, coefficients, degree);
                ans_memory(value, calculations, &count);
                break;
            case 4:
                printf("---삼각함수 계산--- \n");
                printf("삼각함수 종류 \n");
                printf("1. 삼각함수 \n");
                printf("2. 역삼각함수 \n");
                printf("3. 쌍곡선함수 \n");

                printf("삼각함수 종류를 선택하시오. ");
                scanf("%d", &trigon_type);
                printf("\n");
                
                if (trigon_type == 1){
                    printf("삼각함수 종류 \n");
                    printf("1. Sine 함수 \n");
                    printf("2. Cosine 함수 \n");
                    printf("3. Tangent 함수 \n");

                    printf("삼각함수 종류를 선택하시오. ");
                    scanf("%d", &trigon_type2);
                }
                else if (trigon_type == 2){
                    printf("역삼각함수 종류 \n");
                    printf("1. Arcsin 함수 \n");
                    printf("2. Arccos 함수 \n");
                    printf("3. Arctan 함수 \n");

                    printf("역삼각함수 종류를 선택하시오. ");
                    scanf("%d", &trigon_type2);
                }
                else {
                    printf("쌍곡선함수 종류 \n");
                    printf("1. Sinh 함수 \n");
                    printf("2. Cosh 함수 \n");
                    printf("3. Tanh 함수 \n");

                    printf("쌍곡선함수 종류를 선택하시오. ");
                    scanf("%d", &trigon_type2);
                }
                printf("각도를 라디안의 형태로 입력하시오. ");
                scanf("%lf", &radian);

                value = trigon_cal(trigon_type, trigon_type2, radian);
                ans_memory(value, calculations, &count);
                break;
            case 5:
                printf("---지수함수 계산--- \n");
                printf("e의 n승을 입력하시오. ");
                scanf("%d", &x);
                
                value = exponential_cal(x);
                ans_memory(value, calculations, &count);
                break;
            case 6:
                printf("---등차수열 계산--- \n");
                printf("첫째항과 공차를 차례대로 입력하시오. ");
                scanf("%d %d", &a, &d);
                printf("몇 번째 항까지 계산할까요? ");
                scanf("%d", &n);

                value = arithmetic_seq_cal(a, d, n);
                ans_memory(value, calculations, &count);
                break;
            case 7:
                printf("---등비수열 계산--- \n");
                printf("첫째항과 공비를 차례대로 입력하시오. ");
                scanf("%d %d", &a, &r);
                printf("몇 번째 항까지 계산할까요? ");
                scanf("%d", &n);
                
                value = geometric_seq_cal(a, r, n);
                ans_memory(value, calculations, &count);
                break;
            case 8:
    	   		 printf("---팩토리얼 계산--- \n");
    	  		 printf("몇 번째 숫자부터 계산할지 입력하시오. ");
    			 scanf("%d", &fact_num);
    			 value = fact_cal(fact_num);
                             ans_memory(value, calculations, &count);
    			 break;
  		    case 9:
    	  		 printf("---이전 계산 값--- \n");
    	  		 printf("최근 10개의 계산 값을 불러옵니다. \n");
    	  		 for (int i = 0; i < count; i++)
    				 printf("%.2lf \n", calculations[i]);
    	  		 break;
      		default:
    			 printf("올바른 선택이 아닙니다. \n");
 	}
	 printf("%lf \n", value);
 	 choice = closing();
 } while (choice == 1);
 printf("계산기를 종료합니다. \n");

 return 0;
}

double integrate_cal(double x){
	double j=0;
	int i;
	for (i = 0; i < 6; i++)
	{
		j += coefficient[i] * pow(x, i); //다항식의 꼴로 만들어줌
	}
	return j;
}

double log_cal(int log_type, int antilog){
	double result = log_type == 1 ? log(antilog) : log10(antilog);
	return result; 
}

double polynomial_cal(double x1, double* coefficients, int degree) {
    double result = 0;
    for (int i = 0; i <= degree; i++) {
        result += coefficients[i] * pow(x1, i);
    }
    return result;
}

double trigon_cal(int trigon_type, int trigon_type2, double radian){ 
	if (trigon_type == 1) {
		if (trigon_type2 == 1)
			return sin(radian);
		else if (trigon_type2 == 2)
                        return cos(radian);
		else
                        return tan(radian);
	}

	else if (trigon_type == 2) {
                if (trigon_type2 == 1)
                        return asin(radian);
                else if (trigon_type2 == 2)
                        return acos(radian);
                else
                        return atan(radian);
        }

	else{
		if (trigon_type2 == 1)
                        return sinh(radian);
                else if (trigon_type2 == 2)
                        return cosh(radian);
                else
                        return tanh(radian);
	}
}

double exponential_cal(int x){
	return exp(x);
}

double arithmetic_seq_cal(int a, int d, int n){
	return (n*(2*a + (n-1)*d))/2;
}

double geometric_seq_cal(int a, int r, int n){
	return (a*(pow(r, n) - 1))/(r-1);
}

int fact_cal(int num) {
	int result = 1;
	for (int i = 2; i <= num; ++i) {
        	result *= i;
    	}
    	return result;
}

void ans_memory(double value, double calculations[], int *count) {
    // 배열이 가득 찼을 경우
    if (*count == max_memory_size) {
        // 모든 값들을 한 칸씩 앞으로 이동
        for (int i = 0; i < max_memory_size - 1; i++) {
            calculations[i] = calculations[i + 1];
        }
        // 새로운 값을 배열의 마지막에 추가
        calculations[max_memory_size - 1] = value;
    } else {
        // 아직 배열이 가득 차지 않았을 경우
        calculations[*count] = value;
        (*count)++;
    }
}
