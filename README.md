# calculatorparser
calculatorparser
```javascript
/*
expression calculator parser
Author:BenMa

Expr : Term TermTail
Term : Factor FactorTail
TermTail : + Term TermTail | empty
Factor : ( Expr) | number
FactorTail : * Factor FactorTail | empty
Number : [0-9]+
//reference:
http://www.cs.fsu.edu/~lacher/courses/COP4020/fall14/assigns/proj2a.html
*/

var cur = 0;
var tokens = [];

function expr() {
    var t = new term();
    var tt = new termtail(t.value);
    this.value = tt.value;
}

function term() {
    var f = new factor();
    var ft = new factortail(f.value);
    this.value = ft.value;
}

function termtail(subtotal) {
    var tok = tokens[cur];
    if(tok == '+') {
        cur++;
        var t = new term();
        var tt = new termtail(subtotal + t.value);
        this.value = tt.value;
    } else {
        this.value = subtotal;
    }
}

function factor() {
    if(tokens[cur] == '(') {
        cur++;//(
        var e = new expr();
        cur++;//)
        this.value = e.value;
    } else {
        var nu = new number();
        this.value = nu.value;
    }
}

function factortail(subtotal) {
    var tok = tokens[cur];
    if(tok == '*') {
        cur++;
        var f = new factor();
        var ft = new factortail(subtotal*f.value);
        this.value = ft.value;
    } else {
        this.value = subtotal;
    }
}

function number() {
    var tok = tokens[cur];
    this.value =  parseInt(tok);
    cur++;
}

function parse(s) {
    tokens = s.split('');
    cur = 0;
    console.log(tokens);
    var exp = new expr();
    return exp.value;
}

var s = '1+2*3+5*2';
var v = parse(s);
console.log(v);

```
