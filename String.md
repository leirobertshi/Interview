Question: Valid Number

Thought:

1. List all the valid cases:

`"0"` => `true` `" 0.1 "` => `true` `"abc"` => `false` `"1 a"` => `false` `"2e10"` => `true` `" -90e3   "` => `true` `" 1e"` => `false` `"e3"` => `false` `" 6e-1"` => `true` `" 99e2.5 "` => `false` `"53.5e93"` => `true` `" --6 "` => `false` `"-+3"` => `false` `"95a54e53"` => `false`, "+1.e+5" true. 

2. Remove space at the beginning and end of the string
3. Check first char, can be "+", "-", ".", three flags, e, dot and dig
4. check from 2 to size()-2, e and E can't follow "+-" and e can't be the last one char. 
5. 

```c++
class Solution {
public:
    string delspace(string  s){
        int i=0;
        while (s[i]==' '){
            i++;
        }
        int j=s.size()-1;
        while (s[j]==' '){
            j--;
        }
        string res="";
        for (int k=i;k<=j;k++){
            res = res+s[k];
        }
        return res;
    }
     
     
    bool valid(string &s){
        int i=0;
        bool e =false; // check if 'e' exists
        bool dot=false; // check if '.' exists
        bool dig =false;
         
        while (i<s.size()-1){
            if (i==0){ // a number can start with +, -, .
                if (s[i]<'0' || s[i]>'9'){ // if is 0-9 continue
                    if (s[i]=='+' || s[i]=='-' || s[i]=='.'){
                        if (s.size()==1){return false;} // only +, - , . is not a number
                        if (s[i]=='.'){dot=true;}
                    }
                    else {return false;}
                }else{dig=true;}
                i++;continue;
            }
            if (i>0){
                switch (s[i]){
                    case 'e': // e cannot follow +,-
                        if ( e==false && s[i-1]!='+' && s[i-1]!='-' && dig && i!=s.size()-1) {
                            e = true;
                        }else{
                            return false;
                        }
                        break;
                   case 'E': // e cannot follow +,-
                        if ( e==false && s[i-1]!='+' && s[i-1]!='-' && dig && i!=s.size()-1) {
                            e = true;
                        }else{
                            return false;
                        }
                        break;
                   case '+': // + can only occur before e
                        if (s[i-1]=='e' || s[i-1]=='E'){}else{return false;}
                        break;
                   case '-': // - can only occur before e
                        if (s[i-1]=='e' || s[i-1]=='E'){}else{return false;}
                        break;
                   case '.': // . can only occur once and cannot occure after e
                        if (dot==false && e==false){dot=true;}else{return false;}
                        break;
                   default:  // only 0-9 can be valid numbers
                        if (s[i]<'0'||s[i]>'9'){return false;}
                        else{dig = true;}
                        break;
            } 
                i++;continue;
            }
        }
         
        //last dig can only be 0-9, or ., when it is . there is no . and e before
        if (s[s.size()-1]>='0' && s[s.size()-1]<='9'){return true;}
        if (s[s.size()-1]=='.' && !dot && !e && s[s.size()-2]>='0' && s[s.size()-1]<='9') {return true;}        
        return false;
    }
    bool isNumber(string s) {
        // Start typing your C/C++ solution below
        // DO NOT write int main() function
        string s1 = delspace(s); //delete spaces in the front and end, don't delete the spaces in middle.
        if (s1.size()==0){return false;} // if null string, return false;
        return valid(s1);
    }
};
```

