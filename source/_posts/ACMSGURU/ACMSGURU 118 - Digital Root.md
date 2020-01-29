---
title: ACMSGURU 118 - Digital Root
date: 2020-01-29 14:42:33
categories: ACMSGURU
---
# Digital Root

<!--more-->

## Problem Description

Let f(n) be a sum of digits for positive integer n. If f(n) is one-digit number then it is a digital root for n and otherwise digital root of n is equal to digital root of f(n). For example, digital root of 987 is 6. Your task is to find digital root for expression A1*A2*…*AN + A1*A2*…*AN-1 + … + A1*A2 + A1.

## Input

Input file consists of few test cases. There is K (1<=K<=5) in the first line of input. Each test case is a line. Positive integer number N is written on the first place of test case (N<=1000). After it there are N positive integer numbers (sequence A). Each of this numbers is non-negative and not more than 109.

## Output

Write one line for every test case. On each line write digital root for given expression.

## Sample Input

```
1
3 2 3 4
```

## Sample Output

```
5
```

## Solution

```cpp
#include <bits/stdc++.h>

#define MAXN 9999
#define MAXSIZE 10
#define DLEN 4

class BigNum
{
private:
    int a[10000];
    int len;
public:
    BigNum(){ len = 1;memset(a,0,sizeof(a)); }
    BigNum(const int);
    BigNum(const char*);
    BigNum(const BigNum &);
    BigNum &operator=(const BigNum &);

    friend std::istream& operator>>(std::istream&,  BigNum&);
    friend std::ostream& operator<<(std::ostream&,  BigNum&);

    BigNum operator+(const BigNum &) const;
    BigNum operator-(const BigNum &) const;
    BigNum operator*(const BigNum &) const;
    BigNum operator/(const int   &) const;

    BigNum operator^(const int  &) const;
    int    operator%(const int  &) const;
    bool   operator>(const BigNum & T)const;
    bool   operator>(const int & t)const;
};
BigNum::BigNum(const int b)
{
    int c,d = b;
    len = 0;
    memset(a,0,sizeof(a));
    while(d > MAXN)
    {
        c = d - (d / (MAXN + 1)) * (MAXN + 1);
        d = d / (MAXN + 1);
        a[len++] = c;
    }
    a[len++] = d;
}
BigNum::BigNum(const char*s)
{
    int t,k,index,l,i;
    memset(a,0,sizeof(a));
    l=strlen(s);
    len=l/DLEN;
    if(l%DLEN)
        len++;
    index=0;
    for(i=l-1;i>=0;i-=DLEN)
    {
        t=0;
        k=i-DLEN+1;
        if(k<0)
            k=0;
        for(int j=k;j<=i;j++)
            t=t*10+s[j]-'0';
        a[index++]=t;
    }
}
BigNum::BigNum(const BigNum & T) : len(T.len)
{
    int i;
    memset(a,0,sizeof(a));
    for(i = 0 ; i < len ; i++)
        a[i] = T.a[i];
}
BigNum & BigNum::operator=(const BigNum & n)
{
    int i;
    len = n.len;
    memset(a,0,sizeof(a));
    for(i = 0 ; i < len ; i++)
        a[i] = n.a[i];
    return *this;
}
std::istream& operator>>(std::istream & in,  BigNum & b)
{
    char ch[MAXSIZE*4];
    int i = -1;
    in>>ch;
    int l=strlen(ch);
    int count=0,sum=0;
    for(i=l-1;i>=0;)
    {
        sum = 0;
        int t=1;
        for(int j=0;j<4&&i>=0;j++,i--,t*=10)
        {
            sum+=(ch[i]-'0')*t;
        }
        b.a[count]=sum;
        count++;
    }
    b.len =count++;
    return in;

}
std::ostream& operator<<(std::ostream& out,  BigNum& b)
{
    int i;
    out << b.a[b.len - 1];
    for(i = b.len - 2 ; i >= 0 ; i--)
    {
        out.width(DLEN);
        out.fill('0');
        out << b.a[i];
    }
    return out;
}

BigNum BigNum::operator+(const BigNum & T) const
{
    BigNum t(*this);
    int i,big;
    big = T.len > len ? T.len : len;
    for(i = 0 ; i < big ; i++)
    {
        t.a[i] +=T.a[i];
        if(t.a[i] > MAXN)
        {
            t.a[i + 1]++;
            t.a[i] -=MAXN+1;
        }
    }
    if(t.a[big] != 0)
        t.len = big + 1;
    else
        t.len = big;
    return t;
}
BigNum BigNum::operator-(const BigNum & T) const
{
    int i,j,big;
    bool flag;
    BigNum t1,t2;
    if(*this>T)
    {
        t1=*this;
        t2=T;
        flag=0;
    }
    else
    {
        t1=T;
        t2=*this;
        flag=1;
    }
    big=t1.len;
    for(i = 0 ; i < big ; i++)
    {
        if(t1.a[i] < t2.a[i])
        {
            j = i + 1;
            while(t1.a[j] == 0)
                j++;
            t1.a[j--]--;
            while(j > i)
                t1.a[j--] += MAXN;
            t1.a[i] += MAXN + 1 - t2.a[i];
        }
        else
            t1.a[i] -= t2.a[i];
    }
    t1.len = big;
    while(t1.a[t1.len - 1] == 0 && t1.len > 1)
    {
        t1.len--;
        big--;
    }
    if(flag)
        t1.a[big-1]=0-t1.a[big-1];
    return t1;
}

BigNum BigNum::operator*(const BigNum & T) const
{
    BigNum ret;
    int i,j,up;
    int temp,temp1;
    for(i = 0 ; i < len ; i++)
    {
        up = 0;
        for(j = 0 ; j < T.len ; j++)
        {
            temp = a[i] * T.a[j] + ret.a[i + j] + up;
            if(temp > MAXN)
            {
                temp1 = temp - temp / (MAXN + 1) * (MAXN + 1);
                up = temp / (MAXN + 1);
                ret.a[i + j] = temp1;
            }
            else
            {
                up = 0;
                ret.a[i + j] = temp;
            }
        }
        if(up != 0)
            ret.a[i + j] = up;
    }
    ret.len = i + j;
    while(ret.a[ret.len - 1] == 0 && ret.len > 1)
        ret.len--;
    return ret;
}
BigNum BigNum::operator/(const int & b) const
{
    BigNum ret;
    int i,down = 0;
    for(i = len - 1 ; i >= 0 ; i--)
    {
        ret.a[i] = (a[i] + down * (MAXN + 1)) / b;
        down = a[i] + down * (MAXN + 1) - ret.a[i] * b;
    }
    ret.len = len;
    while(ret.a[ret.len - 1] == 0 && ret.len > 1)
        ret.len--;
    return ret;
}
int BigNum::operator %(const int & b) const
{
    int i,d=0;
    for (i = len-1; i>=0; i--)
    {
        d = ((d * (MAXN+1))% b + a[i])% b;
    }
    return d;
}
BigNum BigNum::operator^(const int & n) const
{
    BigNum t,ret(1);
    int i;
    if(n<0)
        exit(-1);
    if(n==0)
        return 1;
    if(n==1)
        return *this;
    int m=n;
    while(m>1)
    {
        t=*this;
        for( i=1;i<<1<=m;i<<=1)
        {
            t=t*t;
        }
        m-=i;
        ret=ret*t;
        if(m==1)
            ret=ret*(*this);
    }
    return ret;
}
bool BigNum::operator>(const BigNum & T) const
{
    int ln;
    if(len > T.len)
        return true;
    else if(len == T.len)
    {
        ln = len - 1;
        while(a[ln] == T.a[ln] && ln >= 0)
            ln--;
        if(ln >= 0 && a[ln] > T.a[ln])
            return true;
        else
            return false;
    }
    else
        return false;
}
bool BigNum::operator >(const int & t) const
{
    BigNum b(t);
    return *this>b;
}

int main() {
    std::ios::sync_with_stdio(false);
    int k;
    std::cin >> k;

    while(k--) {
        int n;
        std::cin >> n;
        BigNum total_sum{0};
        BigNum prefix_sum{1};
        while(n--) {
            int num;
            std::cin >> num;
            prefix_sum = prefix_sum * BigNum{num};
            total_sum = total_sum + prefix_sum;
        }
        std::stringstream string_stream{};
        string_stream << total_sum;
        std::cout.flush();
        int digit_sum = 0;
        for(auto c : string_stream.str()) {
            digit_sum += c - '0';
        }
        while(static_cast<int>(std::log10(digit_sum)) + 1 > 1) {
            int new_sum = 0;
            while(digit_sum != 0) {
                new_sum += digit_sum % 10;
                digit_sum /= 10;
            }
            digit_sum = new_sum;
        }
        std::cout << digit_sum << std::endl;
    }
    return 0;
}
```
