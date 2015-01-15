/*
expression calculator parser v2

*/
```javascript
/*
Expr : Term TermTail
Term : Factor FactorTail
Fator : (Expr) | Number
Number : [0-9]+
TermTail : + Term TermTail | empty
FactorTail : * Factor Factortail | empty;
*/

var tokens = [];
var cur = 0;

function expr() {
    var t =  new term();
    var tt =  new termtail(t.value);
    this.value = tt.value;
}

function term() {
    var f = new factor();
    var ft = new factortail(f.value);
    this.value = ft.value;
}

function factor() {
    var tok = tokens[cur];
    if(tok == '(') {
        cur++;
        var e = new expr();
        cur++;//)
        this.value = e.value;
    } else {
        var n = new number();
        this.value = n.value;
    }
}

function number() {
    var sum = 0;
    while(isFinite(tokens[cur])) {
        sum = sum*10 + parseInt(tokens[cur]);
        cur++;
    }
    this.value = sum;
}

function termtail(pre) {
    if(tokens[cur] == '+') {
        cur++;//+
        var t = new term();
        var tt = new termtail(pre + t.value);
        this.value = tt.value;
    } else {
        this.value = pre;
    }
}

function factortail(pre) {
    if(tokens[cur] == '*') {
        cur++;//*
        var f = new factor();
        var tf = new factortail(pre*f.value);
        this.value = tf.value;
    } else {
        this.value = pre;
    }
}

function parse(s) {
    tokens = s.split('');
    cur = 0;
    var e = new expr();
    console.log(e.value);
    return e.value;
}

parse('115+2*3+5*(1+20)');

```
