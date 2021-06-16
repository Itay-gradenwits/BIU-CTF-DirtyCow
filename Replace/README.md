# PyJail - biuCTF 2021
## WriteUp By DirtyCOW

Let's look at the encode method of the message and put it in the function ``` decode_replace() ``` :
```python
def decode_replace(msg):
    return msg.replace('3', '&L7;').replace('k', 'q^#HA1Sr').replace('t', 'Re/t').replace('a', 'CFe').replace('b', '7CA,').replace('z', '6MJeG').replace('!', 'Gw#').replace('_', '#afyz').replace('Y', 'gsTbP=').replace('^', 'EhM&;!F').replace('^', ',fP').replace('o', 'Yse=z0Q').replace('J', 's?iwm0^-').replace(',', 'P4Y$!!C').replace('H', ';.').replace('y', '-3').replace('k', 'kj').replace('y', '%qG').replace('e', 'JTsHitc').replace('s', '&SY').replace('c', 'PTTA75C').replace(',', '.^Yw6').replace('c', 'JwF+').replace('0', '#K@yt').replace('C', '=OFTstj').replace('z', 'Hii').replace('@', 'uu4R,w').replace('b', 'q:KmBUV@').replace('e', 'ZWwgA').replace('c', 'Fs').replace('4', 'jcMM<K:').replace('B', 'C<G%S').replace('8', '=B@W$z').replace('C', 'ifoPK68').replace('z', '4I/c').replace('W', 'S<CqyN').replace('f', 'y,').replace('u', 'hF<5**Rt').replace('=', 'WU7O<P/;').replace('f', '6s*').replace('n', '0*').replace('U', 'WX').replace('D', '*').replace('$', '3').replace('Q', 'd;;t7wR').replace('h', ';<Cj').replace('>', 'CD98qP').replace('T', 'bvQ0*1.S').replace('D', 'KC').replace('E', '1H').replace('=', '^,kG').replace('a', '-$O#y<').replace('u', '<l$#GV').replace(',', '^J1H?a').replace('Y', 'a8x').replace('o', 'Pwq').replace('7', ',%1h').replace('/', 'Mdxd').replace('J', 'fj#').replace(';', 'rb+B').replace('0', '.*K7l').replace('W', 'p').replace('i', 'q,baM').replace('A', 'L').replace('U', 'N').replace('2', 'kc^^36p%').replace('M', 'N1*3').replace('U', 'evZxd').replace('K', 'NP*').replace('X', 'D./S').replace('K', 'i%').replace('z', '=/,GT').replace('7', 'ACR+fiX').replace('d', 'tk5,lP').replace('5', ':').replace('2', 'nDfK').replace('p', 'j8H').replace('%', 'T8&:e>x').replace('-', 'l04m?HW>').replace('n', 'O1Q').replace('J', '%h').replace('^', '<').replace('?', '/').replace('l', 'ps').replace('w', 'gpG').replace('Z', 'SC$DJ').replace('7', 'JOpX+<').replace('g', 'u%').replace('0', 'XZ').replace('_', '41ZOaC^:').replace('J', 'e').replace('G', 'p?lCw').replace('d', 'iW.Ux-A').replace('4', 'mUN').replace('@', 'o').replace('J', '?I').replace('k', '>6+wP8i').replace('z', 'tyI<myV').replace('P', 'G8tNDEJ.').replace('x', 'i')
```
Now we can encode every character we want.
A very intuitive soloution is to search characters that are prefix of the encode flag and try to predict what is the decode message.
The problem is that there might be some characters that their encode is a prefix of the message so we can't detremine which one of them is a character in the decode message.
Ofcaurse we can just not detremine between those characters and just see which one of them we'll bring us to a decode messgae.
To do that manually is might not be possible so we would have to involve python code.
First lets look at the following funciton: ```chars()```
```python
def chars():
    command = obfuscated_flag = "'3', '&L7;').replace('k', 'q^#HA1Sr').replace('t', 'Re/t').replace('a', 'CFe').replace('b', '7CA,').replace('z', '6MJeG').replace('!', 'Gw#').replace('_', '#afyz').replace('Y', 'gsTbP=').replace('^', 'EhM&;!F').replace('^', ',fP').replace('o', 'Yse=z0Q').replace('J', 's?iwm0^-').replace(',', 'P4Y$!!C').replace('H', ';.').replace('y', '-3').replace('k', 'kj').replace('y', '%qG').replace('e', 'JTsHitc').replace('s', '&SY').replace('c', 'PTTA75C').replace(',', '.^Yw6').replace('c', 'JwF+').replace('0', '#K@yt').replace('C', '=OFTstj').replace('z', 'Hii').replace('@', 'uu4R,w').replace('b', 'q:KmBUV@').replace('e', 'ZWwgA').replace('c', 'Fs').replace('4', 'jcMM<K:').replace('B', 'C<G%S').replace('8', '=B@W$z').replace('C', 'ifoPK68').replace('z', '4I/c').replace('W', 'S<CqyN').replace('f', 'y,').replace('u', 'hF<5**Rt').replace('=', 'WU7O<P/;').replace('f', '6s*').replace('n', '0*').replace('U', 'WX').replace('D', '*').replace('$', '3').replace('Q', 'd;;t7wR').replace('h', ';<Cj').replace('>', 'CD98qP').replace('T', 'bvQ0*1.S').replace('D', 'KC').replace('E', '1H').replace('=', '^,kG').replace('a', '-$O#y<').replace('u', '<l$#GV').replace(',', '^J1H?a').replace('Y', 'a8x').replace('o', 'Pwq').replace('7', ',%1h').replace('/', 'Mdxd').replace('J', 'fj#').replace(';', 'rb+B').replace('0', '.*K7l').replace('W', 'p').replace('i', 'q,baM').replace('A', 'L').replace('U', 'N').replace('2', 'kc^^36p%').replace('M', 'N1*3').replace('U', 'evZxd').replace('K', 'NP*').replace('X', 'D./S').replace('K', 'i%').replace('z', '=/,GT').replace('7', 'ACR+fiX').replace('d', 'tk5,lP').replace('5', ':').replace('2', 'nDfK').replace('p', 'j8H').replace('%', 'T8&:e>x').replace('-', 'l04m?HW>').replace('n', 'O1Q').replace('J', '%h').replace('^', '<').replace('?', '/').replace('l', 'ps').replace('w', 'gpG').replace('Z', 'SC$DJ').replace('7', 'JOpX+<').replace('g', 'u%').replace('0', 'XZ').replace('_', '41ZOaC^:').replace('J', 'e').replace('G', 'p?lCw').replace('d', 'iW.Ux-A').replace('4', 'mUN').replace('@', 'o').replace('J', '?I').replace('k', '>6+wP8i').replace('z', 'tyI<myV').replace('P', 'G8tNDEJ.').replace('x', 'i'"
    commarray = command.split(').replace(')
    commaarray=commarray[10:-3]
    res=[]
    for com in commarray:
        dob = com.split(', ')
        res.append(dob[0][1:-1])
    res.append('{')
    res.append('F')
    res.append('1')
    res.append('#')
    res.append('6')
    res.append('7')
    res.append('9')
    res.append('j')
    res.append('m')
    res.append('q')
    res.append('r')
    res.append('v')
    res.append('}')

    return res
```
This function might look long and difficult but actually it just return an array of all the possible characters in the encode message.
Next, Let's look at the main part of the soloution, the function ```solve(output)```

```python
def solve(output):
if len(output) == 0:
     return ''
for x in chars():
    msg = x
    res = decode_replace(msg)
    if output.startswith(res):
       return x + solve(output[len(res) : len(output)])
 return '***'
```
The funciton is recursive and basically do the solotion from above:
It search for a character that it's encode is a prefix of the encode message And call it self again but this time with encode message without the prefix (the encode of the character).
if we did not find such character we'll return *** to mark that this encode message is not correct.
The stop condition is when the encode message is empty (len == 0).
    
Now Let's look at the full program and run it to see ehat we reccive:
```python
import string

output=',T8&:e>i1hj8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjLG8tNDEJ.jcN1*3N1*3<NG8tNDEJ.*:a8i3!!j8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.Sstjq,baN1*3rb+B<CjF<:**Rtj8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjbvQ.*NG8tNDEJ.*ACR+fiXps*1.SF{1#psXZmUNm/HW>$O#y<y<fj#1H/apsXZmUNm/HW>3Hq,baN1*3q,baN1*3rb+B<Cj#NG8tNDEJ.*rb+B<CjF<:**Rtrb+B<CjF<:**RtjcN1*3N1*3<NG8tNDEJ.*:R<fj#1H/au%pp?lCwytj8H&L,T8&:e>i1hrb+B#psXZmUNm/HW>$O#y<y<fj#1H/apsXZmUNm/HW>3Hq,baN1*3q,baN1*3psXZmUNm/HW>3#NG8tNDEJ.*rb+B<CjF<:**Rtrb+B<CjF<:**RtjcN1*3N1*3<NG8tNDEJ.*:R<fj#1H/au%pp?lCwytrb+B<CjF<:**Rt#psXZmUNm/HW>$O#y<y<fj#1H/apsXZmUNm/HW>3Hq,baN1*3q,baN1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.1t>6+wG8tNDEJ.8i:,psG8tNDEJ..*NG8tNDEJ.*ACR+fiXps*Rfj#bvQ.*NG8tNDEJ.*ACR+fiXps*1.S&Sa8iHq,baN1*3tG8tNDEJ.bvQ.*NG8tNDEJ.*ACR+fiXps*1.SbvQ.*NG8tNDEJ.*ACR+fiXps*1.SL,T8&:e>i1h:j8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjN1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.t#psXZmUNm/HW>$O#y<y<fj#1H/apsXZmUNm/HW>3Hq,baN1*3q,baN1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.#NG8tNDEJ.*rb+B<CjF<:**Rtrb+B<CjF<:**RtjcN1*3N1*3<NG8tNDEJ.*:R<fj#1H/au%pp?lCwyt#psXZmUNm/HW>$O#y<y<fj#1H/apsXZmUNm/HW>3Hq,baN1*3q,baN1*31Rfj#bvQ.*NG8tNDEJ.*ACR+fiXps*1.S&Sa8iHq,baN1*3tG8tNDEJ.bvQ.*NG8tNDEJ.*ACR+fiXps*1.SbvQ.*NG8tNDEJ.*ACR+fiXps*1.SL,T8&:e>i1h:j8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjN1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.t#psXZmUNm/HW>$O#y<y<fj#1H/apsXZmUNm/HW>3Hq,baN1*3q,baN1*3mj8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjFfj#bvQ.*NG8tNDEJ.*ACR+fiXps*1.S&Sa8iHq,baN1*3tG8tNDEJ.bvQ.*NG8tNDEJ.*ACR+fiXps*1.SbvQ.*NG8tNDEJ.*ACR+fiXps*1.SL,T8&:e>i1h:j8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.Sstj.*NG8tNDEJ.*ACR+fiXps*rb+B<CjF<:**Rtj8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjFfj#bvQ.*NG8tNDEJ.*ACR+fiXps*1.S&Sa8iHq,baN1*3tG8tNDEJ.bvQ.*NG8tNDEJ.*ACR+fiXps*1.SbvQ.*NG8tNDEJ.*ACR+fiXps*1.SL,T8&:e>i1h:j8Hj8HD./S,T8&:e>i1hO<G8tNDEJ.N1*3t>6+wG8tNDEJ.8i:,psG8tNDEJ.it>6+wG8tNDEJ.8i:,psG8tNDEJ.rb+BOFbvQ.*NG8tNDEJ.*ACR+fiXps*1.SstjpspspsXZmUNm/HW>3}'

def decode_replace(msg):
    return msg.replace('3', '&L7;').replace('k', 'q^#HA1Sr').replace('t', 'Re/t').replace('a', 'CFe').replace('b', '7CA,').replace('z', '6MJeG').replace('!', 'Gw#').replace('_', '#afyz').replace('Y', 'gsTbP=').replace('^', 'EhM&;!F').replace('^', ',fP').replace('o', 'Yse=z0Q').replace('J', 's?iwm0^-').replace(',', 'P4Y$!!C').replace('H', ';.').replace('y', '-3').replace('k', 'kj').replace('y', '%qG').replace('e', 'JTsHitc').replace('s', '&SY').replace('c', 'PTTA75C').replace(',', '.^Yw6').replace('c', 'JwF+').replace('0', '#K@yt').replace('C', '=OFTstj').replace('z', 'Hii').replace('@', 'uu4R,w').replace('b', 'q:KmBUV@').replace('e', 'ZWwgA').replace('c', 'Fs').replace('4', 'jcMM<K:').replace('B', 'C<G%S').replace('8', '=B@W$z').replace('C', 'ifoPK68').replace('z', '4I/c').replace('W', 'S<CqyN').replace('f', 'y,').replace('u', 'hF<5**Rt').replace('=', 'WU7O<P/;').replace('f', '6s*').replace('n', '0*').replace('U', 'WX').replace('D', '*').replace('$', '3').replace('Q', 'd;;t7wR').replace('h', ';<Cj').replace('>', 'CD98qP').replace('T', 'bvQ0*1.S').replace('D', 'KC').replace('E', '1H').replace('=', '^,kG').replace('a', '-$O#y<').replace('u', '<l$#GV').replace(',', '^J1H?a').replace('Y', 'a8x').replace('o', 'Pwq').replace('7', ',%1h').replace('/', 'Mdxd').replace('J', 'fj#').replace(';', 'rb+B').replace('0', '.*K7l').replace('W', 'p').replace('i', 'q,baM').replace('A', 'L').replace('U', 'N').replace('2', 'kc^^36p%').replace('M', 'N1*3').replace('U', 'evZxd').replace('K', 'NP*').replace('X', 'D./S').replace('K', 'i%').replace('z', '=/,GT').replace('7', 'ACR+fiX').replace('d', 'tk5,lP').replace('5', ':').replace('2', 'nDfK').replace('p', 'j8H').replace('%', 'T8&:e>x').replace('-', 'l04m?HW>').replace('n', 'O1Q').replace('J', '%h').replace('^', '<').replace('?', '/').replace('l', 'ps').replace('w', 'gpG').replace('Z', 'SC$DJ').replace('7', 'JOpX+<').replace('g', 'u%').replace('0', 'XZ').replace('_', '41ZOaC^:').replace('J', 'e').replace('G', 'p?lCw').replace('d', 'iW.Ux-A').replace('4', 'mUN').replace('@', 'o').replace('J', '?I').replace('k', '>6+wP8i').replace('z', 'tyI<myV').replace('P', 'G8tNDEJ.').replace('x', 'i')

def chars():
    command = obfuscated_flag = "'3', '&L7;').replace('k', 'q^#HA1Sr').replace('t', 'Re/t').replace('a', 'CFe').replace('b', '7CA,').replace('z', '6MJeG').replace('!', 'Gw#').replace('_', '#afyz').replace('Y', 'gsTbP=').replace('^', 'EhM&;!F').replace('^', ',fP').replace('o', 'Yse=z0Q').replace('J', 's?iwm0^-').replace(',', 'P4Y$!!C').replace('H', ';.').replace('y', '-3').replace('k', 'kj').replace('y', '%qG').replace('e', 'JTsHitc').replace('s', '&SY').replace('c', 'PTTA75C').replace(',', '.^Yw6').replace('c', 'JwF+').replace('0', '#K@yt').replace('C', '=OFTstj').replace('z', 'Hii').replace('@', 'uu4R,w').replace('b', 'q:KmBUV@').replace('e', 'ZWwgA').replace('c', 'Fs').replace('4', 'jcMM<K:').replace('B', 'C<G%S').replace('8', '=B@W$z').replace('C', 'ifoPK68').replace('z', '4I/c').replace('W', 'S<CqyN').replace('f', 'y,').replace('u', 'hF<5**Rt').replace('=', 'WU7O<P/;').replace('f', '6s*').replace('n', '0*').replace('U', 'WX').replace('D', '*').replace('$', '3').replace('Q', 'd;;t7wR').replace('h', ';<Cj').replace('>', 'CD98qP').replace('T', 'bvQ0*1.S').replace('D', 'KC').replace('E', '1H').replace('=', '^,kG').replace('a', '-$O#y<').replace('u', '<l$#GV').replace(',', '^J1H?a').replace('Y', 'a8x').replace('o', 'Pwq').replace('7', ',%1h').replace('/', 'Mdxd').replace('J', 'fj#').replace(';', 'rb+B').replace('0', '.*K7l').replace('W', 'p').replace('i', 'q,baM').replace('A', 'L').replace('U', 'N').replace('2', 'kc^^36p%').replace('M', 'N1*3').replace('U', 'evZxd').replace('K', 'NP*').replace('X', 'D./S').replace('K', 'i%').replace('z', '=/,GT').replace('7', 'ACR+fiX').replace('d', 'tk5,lP').replace('5', ':').replace('2', 'nDfK').replace('p', 'j8H').replace('%', 'T8&:e>x').replace('-', 'l04m?HW>').replace('n', 'O1Q').replace('J', '%h').replace('^', '<').replace('?', '/').replace('l', 'ps').replace('w', 'gpG').replace('Z', 'SC$DJ').replace('7', 'JOpX+<').replace('g', 'u%').replace('0', 'XZ').replace('_', '41ZOaC^:').replace('J', 'e').replace('G', 'p?lCw').replace('d', 'iW.Ux-A').replace('4', 'mUN').replace('@', 'o').replace('J', '?I').replace('k', '>6+wP8i').replace('z', 'tyI<myV').replace('P', 'G8tNDEJ.').replace('x', 'i'"
    commarray = command.split(').replace(')
    commaarray=commarray[10:-3]
    res=[]
    for com in commarray:
        dob = com.split(', ')
        res.append(dob[0][1:-1])
    res.append('{')
    res.append('F')
    res.append('1')
    res.append('#')
    res.append('6')
    res.append('7')
    res.append('9')
    res.append('j')
    res.append('m')
    res.append('q')
    res.append('r')
    res.append('v')
    res.append('}')

    return res

def solve(output):
    if len(output) == 0:
        return ''
    for x in chars():
        msg = x
        res = decode_replace(msg)
        if output.startswith(res):
            return x + solve(output[len(res) : len(output)])
    return '***'


print(solve(output))
```

after running we get ``` biuCTF{1_h0p3_y0u_d1dnt_d0_1t_manually} ``` witch is exactly the flag.
