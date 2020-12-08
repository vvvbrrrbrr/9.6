# 9.6
#include <stdio.h>
#include <stdlib.h>


int** funk(int n, int k)
{
    int** a = malloc(n*sizeof(int*));
    for(int i=0; i<n; i++)
        a[i]=malloc(k*sizeof(int));
    return a;
}
float** funkf(int n, int k)
{
    float** a = calloc(n, sizeof(float*));
    for(int i=0; i<n; i++)
        a[i]=calloc(k, sizeof(float));
    return a;
}

int nod (int x, int y)
{
    if (y==0)
        return x;
    else
        return nod(y, x%y);
}
int nok (int x, int y)
{
    return (x*y)/nod(x, y);
}

void pomenyatt (int*a, int*b)
{
    int t=*a;
    *a=*b;
    *b=t;
}
void pomenyatb (float*a, float*b)
{
    float t=*a;
    *a=*b;
    *b=t;
}


float** treugV2(int** a, float** b, int n)
{
    for(int j=0; j<n; j++)
    {
        int i=j;
        while(a[i][j]==0)
        {
            i++;
            if(i>=n)
            {
                printf("\nopredelitelb raven nulu\n");
                return b;
            }
        }

        for(int k=j; k<n; k++)
            pomenyatt(&a[j][k], &a[i][k]);
        for(int k=0; k<n; k++)
            pomenyatb(&b[j][k], &b[i][k]);
        for(int k=0; k<j; k++)
        {
            if(a[k][j]!=0){
            int p = nok(a[j][j], a[k][j]);
            for(i=n-1; i>=0; i--)
                b[k][i]=(b[k][i]*p/a[k][j])+b[j][i]*(-p/a[j][j]);
            for(i=n-1; i>=j; i--)
                a[k][i]=(a[k][i]*p/a[k][j])+a[j][i]*(-p/a[j][j]);
            }
        }
        for(int k=j+1; k<n; k++)
        {
            if(a[k][j]!=0){
            int p = nok(a[j][j], a[k][j]);
            for(i=n-1; i>=0; i--)
                b[k][i]=(b[k][i]*p/a[k][j])+b[j][i]*(-p/a[j][j]);
            for(i=n-1; i>=j; i--)
                a[k][i]=(a[k][i]*p/a[k][j])+a[j][i]*(-p/a[j][j]);
            }
        }
    }
    printf("\ndiagonalnaya matritsa:\n");
    for (int j=0; j<n; j++)
    {
        for (int i=0; i<n; i++)
            printf("%d ", a[j][i]);
        printf("\n");
    }
    printf("\nzagotovochka dlya obratnoy matritsy2:\n");
    for (int j=0; j<n; j++)
    {
        for (int i=0; i<n; i++)
            printf("%f ", b[j][i]);
        printf("\n");
    }
    printf("\n");
    for(int j=0; j<n; j++)
        for(int i=0; i<n; i++)
            b[j][i]=b[j][i]/a[j][j];
    return b;
}

void proverka(int**aa, float**b, int n)
{
    float** e= funkf(n,n);
    for(int j=0; j<n; j++)
        for(int i=0; i<n; i++)
             for(int k=0; k<n; k++)
                e[j][i]=e[j][i]+(aa[j][k]*b[k][j]);
    for (int j=0; j<n; j++)
    {
        for (int i=0; i<n; i++)
            printf("%f ", e[j][i]);
        printf("\n");
    }
    printf("\n");
}

int main()
{
    int n, i, j;
    printf("Vvedite kol-vo peremennyih \n");
    scanf("%d", &n);
    int** a = funk(n, n);
    for (j=0; j<n; j++)
        for (i=0; i<n; i++)
        {
              scanf("%d", &a[j][i]); 
        }
    printf("matritsa sistemy:\n");
    for (j=0; j<n; j++)
    {
        for (i=0; i<n; i++)
            printf("%d ", a[j][i]);
        printf("\n");
    }
    int** aa = funk(n, n);
    for (j=0; j<n; j++)
        for (i=0; i<n; i++)
        {
            aa[j][i]=a[j][i];
        }
    float** b = funkf(n,n);
    for (j=0; j<n; j++)
        b[j][j]=1.0;
    printf("zagotovochka obratnoy matritsy:\n");
    for (j=0; j<n; j++)
    {
        for (i=0; i<n; i++)
            printf("%f ", b[j][i]);
        printf("\n");
    }
    printf("\n");

    b = treugV2(a, b, n);

    printf("obratnaya matritsa:\n");
    for (j=0; j<n; j++)
    {
        for (i=0; i<n; i++)
            printf("%f ", b[j][i]);
        printf("\n");
    }
    printf("\n");
    printf("proverka (rezultat peremnozheniya):\n");
     proverka(aa, b, n);
    return 0;
}
