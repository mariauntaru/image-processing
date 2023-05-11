# image-processing
applying a mask on an image, generated with AI
Untaru Maria 332AB
Coral truck
Am folosit modelul generativ Stable Diffusion, din cadrul keras-cv generand o serie de imagini 
pe baza unei propozitii, cu datele asignate „coral truck”. Imaginile generate sunt de calitate 
buna, insa, la fiecare generare a unui batch de cate 3 fotografii, la cel putin una dintre ele nu 
este respectat in totalitate prompt-ul. La fiecare generare, una dintre fotografii a fost identificata 
ca „bus”, nu „truck”, si culoarea nu a fost respectata cu exactitate. Dupa generare, care a fost un 
proces relativ lent, cu ajutorul bibliotecii PIL (Python Imaging Library), am salvat imaginile in 
format png.
Yolov5 e o retea de detectie a obiectelor, rapida si precisa, ce salveaza fotografia inserata cu un 
marcaj asupra obiectului identificat si denumirea acestuia, si un fisier text corespunzator 
imaginii unde se afla niste coordonate, ce reprezinta: indexul clasei la care a fost asignat 
obiectul detectat, coordonata x a colțului stânga-sus al casetei delimitatoare care înconjoară 
obiectul, coordonata y a colțului stânga-sus al casetei delimitatoare care înconjoară obiectul, 
lățimea casetei delimitatoare care înconjoară obiectul, si înălțimea casetei delimitatoare care 
înconjoară obiectul.
Apoi, citind coordonatele din fisierele rezultate, am decupat cu ajutorul functiei crop din 
biblioteca PIL, doar bucata din fotografia generata ce contine obiectul dorit. Am avut de 
efectuat trecerea in spațiul HSV, unde HSV înseamnă nuanță (Hue), saturație (Saturation) și 
valoare (Value), și reprezintă culorile pe baza acestor trei componente, ce e benefica pentru 
segmentarea si analiza bazata pe culori. Conversia din RGB in HSV a fost realizata cu functia 
convert, care pastreaza valorile pixelilor, pastrand valorile intr-un array de tip numpy. In spatiul 
de culoare HSV, pentru a defini o masca avem nevoie de un prag superior si unul inferior 
(vectorii „color_lower” si „color_upper”). Obtinem masca de culoare utilizand functia 
cv2.inRange, care creeaza masca ce seteaza pixelii ce corespund intervalului de culoare 
specificat la alb (255), si restul la negru (0). Apoi, cu ajutorul functiei cv2.bitwise_and, efectuam 
o operatie de si logic, intre imaginea convertita la spatiul de culoare HSV si masca, colorand 
zona ce anterior era alba, devenind vizibila in culoarea cautata, restul imaginii ramanand setat la 
negru. Apoi, imaginea rezultata, e convertita in format RGB, folosind din nou functia convert, si 
salvata.
Pentru a converti masca într-o imagine cu transparență, folosim o abordare în care masca binara
este utilizată ca canal alfa al imaginii rezultate. Pentru fiecare pixel din imaginea originală, 
canalul alfa (ce reprezinta transparența imaginii) va fi 0 pentru pixelii care aparțin fundalului și 
255 pentru pixelii care aparțin obiectului de interes.
Untaru Maria 332AB
Funcția transparent_fcn primește imaginea originală și masca binară ca argumente și creează o 
imagine cu patru canale (RGB + canal alfa) în care valorile RGB sunt copiate din imaginea 
originală, iar canalul alfa este setat utilizând masca binară. Imaginea rezultată este apoi salvată 
în formatul png. Pentru a utiliza această funcție, imaginea originală trebuie să fie în formatul 
numpy array, așa că se folosește np.array pentru a obține reprezentarea tabloului numpy al 
imaginii.
