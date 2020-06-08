# Bermuda-Triangle-Puzzle-Project
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Fri Jun  5 14:35:51 2020

@author: denalipray
"""


## Current Blueprint ##
# 1. find the possibilities for each small combo (beg, mid, end)
#    result: 9 small sections
#    * May have to come back and further restrict based on piece #s or
#      repeated sections
#    * May have to determine piece 3 and piece 5 based on existing
#      possibilities restricted by set3/4 and set6/7
# 2. find all permutations of the nine sections to create sequences
# 3. find the middle piece in each sequence based on the index positions
# 4. determine if the middle piece is an actual possibility based on A
# 5. check sequences against piece restrictions using data frame to 
#    map each tuple to its piece number(s) and determine which sequence
#    has one of each piece
   
# code pieces in sets of 2
pieces = ["wy", "yg", "gw",
          "gy", "ye", "eg",
          "ew", "we", "ee",
          "wk", "ke", "ew",
          "wr", "ry", "yw",
          "kr", "rg", "gk",
          "yk", "kg", "gy",
          "wg", "gr", "rw",
          "kr", "rg", "gk",
          "kk", "kg", "gk",
          "ww", "we", "ew",
          "ry", "yg", "gr",
          "kg", "gr", "rk",
          "wy", "yg", "gw",
          "ky", "ye", "ek",
          "ew", "wk", "ke"]


# every piece of 3 colors in each of the 3 orientations
A =      ["wyg", "ygw", "gwy",
          "gye", "yeg", "egy",
          "ewe", "wee", "eew",
          "wke", "kew", "ewk",
          "wry", "ryw", "ywr",
          "krg", "rgk", "gkr",
          "ykg", "kgy", "gyk",
          "wgr", "grw", "rwg",
          "krg", "rgk", "gkr",
          "kkg", "kgk", "gkk",
          "wwe", "wew", "eww",
          "ryg", "ygr", "gry",
          "kgr", "grk", "rkg",
          "wyg", "ygw", "gwy",
          "kye", "yek", "eky",
          "ewk", "wke", "kew"]

#print(A[0])

# make A into an array but with the number of pieces (to show dup pieces)
import numpy as np
A_col1 =    A[0],A[3],A[6],A[9],A[12],A[15], \
            A[18],A[21],A[27],A[30],A[33],\
            A[36],A[42]
            
A_col2 =    A[1],A[4],A[7],A[10],A[13],A[16], \
            A[19],A[22],A[28],A[31],A[34],\
            A[37],A[43]
            
A_col3 =    A[2],A[5],A[8],A[11],A[14],A[17], \
            A[20],A[23],A[29],A[32],A[35],\
            A[38],A[44]
            
A_col4 =    [2,1,1,2,1,2,1,1,1,1,1,1,1]
A_array=np.column_stack((A_col1, A_col2, A_col3, A_col4))

# Building Block Approach

# zeros here represent a two-piece set
s = ["g", "k", 0, 0,
     "g", 0, 0, "r",
     0, 0, "e", "w",
     0, 0, "r", 0, 0,
     "w", 0, 0, "y", 
     "g", 0, 0, "g", 
     0, 0, "w", 0, 0]

################################### SIDE 1 ###################################
### Finding First Section Piece Combinations ###

# find possible set 1
s1 = []
for x in range(0, 48):
  if s[1] == pieces[x][0]: #s[1] is "k" in the sequence s
        s1.append(pieces[x]) 
        s1 = list(set(s1)) #remove duplicates
# loop through all pieces (sets) and if the piece starts with a "k",
# add it to the list of possible set 1s

#print(s1)

# piece 1 (sets 0/1) is corner piece, must restrict to existing pieces
# does s[0]+s1 exist in A where s[0] is "g"
piece1 = []
s0 = []
s1_restrict = []
for x in range(0, len(s1)):
    for y in range(0,48):
        if s[0]+s1[x] == A[y]: #s[0] is "g"
            s0.append(A[y][0] + A[y][1])
            s0 = list(set(s0))
            s1_restrict.append(s1[x])
            s1_restrict = list(set(s1_restrict))
            piece1.append(A[y])
            piece1 = list(set(piece1))     
        #else: print("No match")

s1 = s1_restrict # replace s1 with  possibilities after corner restriction

# find set 3
s3 = []
for x in range(0, 48):
   if s[4] == pieces[x][1]:
       s3.append(pieces[x])
       s3 = list(set(s3))
#print(s3)

# permute the combos of the restricted end sets
c=[x + y for x in s1 for y in s3]
#print(c)

# find possible middle set based on outer sets
c_list = []
pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove duplicates
      #else: print("No match")

s2 = c_list  
#print(s3)


### Finding Second Section Piece Combinations ###
# find possible set 4
s4 = []
for x in range(0, 48):
  if s[4] == pieces[x][0]: 
        s4.append(pieces[x])
        s4 = list(set(s4))
#print(s4)

# find possible set 6
s6 = []
for x in range(0, 48):
   if s[7] == pieces[x][1]:
       s6.append(pieces[x])
       s6 = list(set(s6)) # remove duplicates
#print(s6)

# permute the combos of the restricted end sets
c=[x + y for x in s4 for y in s6]
#print(c)

# find possible middle piece based on outer sets
c_list = []
# pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove duplicates

s5 = c_list           
#print(s5)


### Finding Third Section Piece Combinations ###
# find possible set 7
s7 = []
for x in range(0, 48):
  if s[7] == pieces[x][0]: 
        s7.append(pieces[x])
        s7 = list(set(s7)) # remove duplicates
#print(s7)

# find possible piece 9
s9 = []
for x in range(0, 48):
   if s[10] == pieces[x][1]:
       s9.append(pieces[x])
       s9 = list(set(s9))
#print(s9)

## since piece 7 (sets 9/10) is corner piece, must restrict to existing pieces
# does piece 7 exist in A
piece7 = []
s9_restrict = []
s10 = []
for x in range(0, len(s9)):
    for y in range(0,48):
        if s9[x]+"w" == A[y]: 
            piece7.append(A[y])
            piece7 = list(set(piece7))
            s9_restrict.append(A[y][0]+ A[y][1])
            s9_restrict = list(set(s9_restrict))
            s10.append(A[y][1]+ A[y][2])
            s10 = list(set(s10))
        #else: print("No match")
        
#print(s9_restrict)
#print(s10)
#print(piece7)
s9 = s9_restrict # replace with the possibilities after corner restriction

# permute the combos of the restricted end pieces
c=[x + y for x in s7 for y in s9]
#print(c)

# find possible middle piece based on outer piece restrictions
c_list = []
#pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove duplicates

s8 = c_list              
#print(s8)


# print(s0)
# print(s1)
# print(s2)
# print(s3)
# print(s4)
# print(s5)
# print(s6)
# print(s7)
# print(s8)
# print(s9)
# print(s10)
# print(piece1)
# #print(piece3)
# #print(piece5)
# print(piece7)

# permute the combos of side 1
c=[a +"-"+ b +"-"+ c +"-"+ d +"-"+ e +"-"+ f +"-"+ g +"-"+ h +"-"+ i
   +"-"+ j +"-"+ k
   
   for a in s0
   for b in s1
   for c in s2
   for d in s3
   for e in s4
   for f in s5
   for g in s6
   for h in s7
   for i in s8
   for j in s9
   for k in s10]

# determine which side 1 possibilities have the proper matching sequence
# between the end of one set and the beginning of the next
side1 = []
for x in range (0,len(c)):
    if c[x][1]==c[x][3] and c[x][4]==c[x][6] and c[x][7]==c[x][9] and \
        c[x][10]==c[x][12] and c[x][13]==c[x][15] and c[x][16]==c[x][18] \
        and c[x][19]==c[x][21] and c[x][22]==c[x][24] and c[x][25]==c[x][27] \
        and c[x][28]==c[x][30]:
        side1.append(c[x])
        side1 = list(set(side1)) # remove duplicates
#print(len(side1))


# determine which sequences are impossible because of the restrictions on
# pieces 3 and 5 (since pieces 3 and 5 consist of two sets both on side 1
# thus each pair of sets must be able to exist on a singular piece in A

side1_restrict = []
for x in range (0,len(side1)):
    for y in range (0,len(A)):
        if side1[x][9]+side1[x][10]+side1[x][13]==A[y]:
            side1_restrict.append(side1[x])
            side1_restrict = list(set(side1_restrict))  # remove duplicates
#print(len(side1_restrict))
#print(side1_restrict)
side1 = side1_restrict

side1_restrict = []
for x in range (0,len(side1)):
    for y in range (0,len(A)):
        if side1[x][18]+side1[x][19]+side1[x][22] \
            ==A[y]:
            side1_restrict.append(side1[x])
            side1_restrict = list(set(side1_restrict))  # remove duplicates
#print(len(side1_restrict2))
#print(side1_restrict2)

side1 = side1_restrict


################################### SIDE 2 ###################################
### Finding Fourth Section Piece Combinations ###

# find set 11
#print(s10)
s11 = []   
# since piece 7 possibilities were already determined at the end of
# side 1 code, they are reused here to get set 11
for x in range(0, len(piece7)):
    s11.append(piece7[x][2]+piece7[x][0])
#print(s11)

# find set 13
s13 = []
for x in range(0, 48):
   if s[14] == pieces[x][1]:
       s13.append(pieces[x])
       s13 = list(set(s13))
#print(s13)

# permute the combos of the restricted end sets
c=[x + y for x in s11 for y in s13]
#print(c)

# find possible middle set based on outer sets
c_list = []
pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove dups
      #else: print("No match")
s12 = c_list     

### Finding Fifth Section Piece Combinations ###
# find possible set 14
s14 = []
for x in range(0, 48):
  if s[14] == pieces[x][0]: 
        s14.append(pieces[x])
        s14 = list(set(s14))
#print(s14)

# find possible set 16
s16 = []
for x in range(0, 48):
   if s[17] == pieces[x][1]:
       s16.append(pieces[x])
       s16 = list(set(s16))
#print(s16)

# permute the combos of the restricted end sets
c=[x + y for x in s14 for y in s16]
#print(c)

# find possible middle piece based on outer sets
c_list = []
# pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove dups

s15 = c_list         
#print(s15)

### Finding Sixth Section Piece Combinations ###
# find possible set 17
s17 = []
for x in range(0, 48):
  if s[17] == pieces[x][0]: 
        s17.append(pieces[x])
        s17 = list(set(s17))
#print(s17)

# find possible piece 9
s19 = []
for x in range(0, 48):
   if s[20] == pieces[x][1]:
       s19.append(pieces[x])
       s19 = list(set(s19))
#print(s19)

# since piece 12 (sets 19/20) is corner piece, must restrict to existing pieces
# does piece 12 exist in A
piece12 = []
s19_restrict = []
s20 = []
for x in range(0, len(s19)):
    for y in range(0,48):
        if s19[x]+"g" == A[y]: 
            piece12.append(A[y])
            piece12 = list(set(piece12))
            s19_restrict.append(A[y][0]+ A[y][1])
            s19_restrict = list(set(s19_restrict))
            s20.append(A[y][1]+ A[y][2])
            s20 = list(set(s20))
        #else: print("No match")
        
#print(s19_restrict)
#print(s20)
#print(piece12)
s19 = s19_restrict # replace with the possibilities after corner restriction

# permute the combos of the restricted end pieces
c=[x + y for x in s17 for y in s19]
print(c)

# find possible middle piece based on outer piece restrictions
c_list = []
#pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove dups

s18 = c_list              
#print(s18)


# print(s10)
# print(s11)
# print(s12)
# print(s13)
# print(s14)
# print(s15)
# print(s16)
# print(s17)
# print(s18)
# print(s19)
# print(s20)

# print("Pieces:")
# print(piece1)
# print(piece7)
# print(piece12)


# permute the combos of side 2
c=[a +"-"+ b +"-"+ c +"-"+ d +"-"+ e +"-"+ f +"-"+ g +"-"+ h +"-"+ i
   +"-"+ j +"-"+ k
   
   for a in s10
   for b in s11
   for c in s12
   for d in s13
   for e in s14
   for f in s15
   for g in s16
   for h in s17
   for i in s18
   for j in s19
   for k in s20]

side2 = []
for x in range (0,len(c)):
    if c[x][1]==c[x][3] and c[x][4]==c[x][6] and c[x][7]==c[x][9] and \
        c[x][10]==c[x][12] and c[x][13]==c[x][15] and c[x][16]==c[x][18] \
        and c[x][19]==c[x][21] and c[x][22]==c[x][24] and c[x][25]==c[x][27] \
        and c[x][28]==c[x][30]:
        side2.append(c[x])
        side2 = list(set(side2)) # remove dups
        
#print(len(side2))

# determine which sequences are impossible because of the restrictions on
# pieces 8 and 10 (since pieces 8 and 10 consist of two sets each on side 2
# thus each pair of sets must be able to exist on a singular piece in A

side2_restrict = []
for x in range (0,len(side2)):
    for y in range (0,len(A)):
        if side2[x][9]+side2[x][10]+side2[x][13]==A[y]:
            side2_restrict.append(side2[x])
            side2_restrict = list(set(side2_restrict)) # remove dups
#print(len(side2_restrict))
#print(side2_restrict)
side2 = side2_restrict

side2_restrict = []
for x in range (0,len(side2)):
    for y in range (0,len(A)):
        if side2[x][18]+side2[x][19]+side2[x][22]==A[y]:
            side2_restrict.append(side2[x])
            side2_restrict = list(set(side2_restrict)) # remove dups
#print(len(side2_restrict2))
#print(side2_restrict2)

side2 = side2_restrict

#=============================================================================

################################### SIDE 3 ###################################
### Finding Seventh Section Piece Combinations ###

#print(s20)

# find set 21
s21 = []   
# since piece 12 possibilities were already determined at the end of
# side 2 code, they are reused here to get set 21
for x in range(0, len(piece12)):
    s21.append(piece12[x][2]+piece12[x][0])
#print(s21)

# find set 23
s23 = []
for x in range(0, 48):
   if s[24] == pieces[x][1]:
       s23.append(pieces[x])
       s23 = list(set(s23))
#print(s23)

# permute the combos of the restricted end sets
c=[x + y for x in s21 for y in s23]
#print(c)

# find possible middle set based on outer sets
c_list = [] 
pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove dups
      #else: print("No match")
s22 = c_list     

### Finding Eighth Section Piece Combinations ###
# find possible set 14
s24 = []
for x in range(0, 48):
  if s[24] == pieces[x][0]: 
        s24.append(pieces[x])
        s24 = list(set(s24))
#print(s24)

# find possible set 16
s26 = []
for x in range(0, 48):
   if s[27] == pieces[x][1]:
       s26.append(pieces[x])
       s26 = list(set(s26))
#print(s26)

# permute the combos of the restricted end sets
c=[x + y for x in s24 for y in s26]
#print(c)

# find possible middle piece based on outer sets
c_list = []
# pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove dups

s25 = c_list         
#print(s25)

### Finding Ninth Section Piece Combinations ###
# find possible set 27
s27 = []
for x in range(0, 48):
  if s[27] == pieces[x][0]: 
        s27.append(pieces[x])
        s27 = list(set(s27))
#print(s27)

# find possible piece 29
# since piece 1 (sets 29/0) possibilities were already determined at the
# beginning of side 1 code, they are reused here to get set 29
#print(piece1)
#print(s0)

s29 = []
for x in range(0, len(piece1)):
    s29.append(piece1[x][2]+piece1[x][0])
#print(s29)

# permute the combos of the restricted end pieces
c=[x + y for x in s27 for y in s29]
#print(c)

# find possible middle piece based on outer piece restrictions
c_list = []
#pieces_used = []
for x in range (0, len(c)):
    for y in range(0,48):
      if c[x][1] == pieces[y][0] and c[x][2] == pieces[y][1]:
            c_list.append(pieces[y])
            c_list = list(set(c_list)) # remove dups

s28 = c_list              
#print(s28)


# print(s20)
# print(s21)
# print(s22)
# print(s23)
# print(s24)
# print(s25)
# print(s26)
# print(s27)
# print(s28)
# print(s29)
# print(s0)

# print("Corner Pieces:")
# print(piece1)
# print(piece7)
# print(piece12)


# permute the combos of side 3
c=[a +"-"+ b +"-"+ c +"-"+ d +"-"+ e +"-"+ f +"-"+ g +"-"+ h +"-"+ i
   +"-"+ j +"-"+ k
   
   for a in s20
   for b in s21
   for c in s22
   for d in s23
   for e in s24
   for f in s25
   for g in s26
   for h in s27
   for i in s28
   for j in s29
   for k in s0]

side3 = []
for x in range (0,len(c)):
    if c[x][1]==c[x][3] and c[x][4]==c[x][6] and c[x][7]==c[x][9] and \
        c[x][10]==c[x][12] and c[x][13]==c[x][15] and c[x][16]==c[x][18] \
        and c[x][19]==c[x][21] and c[x][22]==c[x][24] and c[x][25]==c[x][27] \
        and c[x][28]==c[x][30]:
        side3.append(c[x])
        side3 = list(set(side3)) # remove dups
        
#print(len(side3))

# determine which sequences are impossible because of the restrictions on
# pieces 13 and 15 (since pieces 13 and 15 consist of two sets each on side 3
# thus each pair of sets must be able to exist on a singular piece in A

side3_restrict = []
for x in range (0,len(side3)):
    for y in range (0,len(A)):
        if side3[x][9]+side3[x][10]+side3[x][13]==A[y]:
            side3_restrict.append(side3[x])
            side3_restrict = list(set(side3_restrict)) # remove dups
#print(len(side3_restrict))
#print(side2_restrict)
side3 = side3_restrict

side3_restrict = []
for x in range (0,len(side3)):
    for y in range (0,len(A)):
        if side3[x][18]+side3[x][19]+side3[x][22]==A[y]:
            side3_restrict.append(side3[x])
            side3_restrict = list(set(side3_restrict)) # remove dups
#print(len(side3_restrict))
#print(side2_restrict)
side3 = side3_restrict

# some side 1 sequences use duplicate pieces because the current method
# treated sets as always being on separate pieces, which isn't quite true
# for example, in the sequence
# 'gk-kr-ry-yg-gw-ww-wr-ry-yw-we-ew--ew-ww-wg-gr-rw-wg-gw-wy-yw-wy-yg--
# yg-gw-wk-kg-gk-ky-yw-wr-rk-kg-gk'
# in side 1, the ry piece appears on the same piece as the wr piece and therefore
# would have to be used in two different positions on the same side which
# is not possible

#=============================================================================
#SIDE 1
#=============================================================================

## look up each of the known pieces and add 1 in the last column of that row
## to indicate if the piece is used in the current sequence

test_side1=[]
## start loop over all side 1 sequences
for k in range(0,len(side1)):
    ### start of loop for particular side 1 sequence
    # determine options for piece 2, piece 4, piece 6 which each have one unknown
    # piece 2
    piece2=[]
    for y in range(0, len(A)):
        if side1[k][6]+side1[k][7]==A[y][0]+A[y][1]:
            piece2.append(A[y][0]+A[y][1]+A[y][2])
            
    piece4=[]
    for y in range(0, len(A)):
        if side1[k][15]+side1[k][16]==A[y][0]+A[y][1]:
            piece4.append(A[y][0]+A[y][1]+A[y][2])
            
    piece6=[]
    for y in range(0, len(A)):
        if side1[k][24]+side1[k][25]==A[y][0]+A[y][1]:
            piece6.append(A[y][0]+A[y][1]+A[y][2])
            
    # permute the options, throw out duplicates, 
    #and add to the end of the side 1, store in test_side1
    unknowns=[","+ x + "-" + y + "-" + z for x in piece2 for y in piece4 for z in piece6]
    unknowns = list(set(unknowns)) # remove dups
    
    for j in range(0,len(unknowns)):
        test_side1.append(side1[k]+unknowns[j])
### end of loop that goes through each side's different options for unknowns

possible_side1=[] # reset possible_side1
## loop through this code to determine which pieces the test sequence
## (i.e. side 1 sequences + the specifications for pieces 2,4,6) uses
for i in range(0, len(test_side1)):
    #reset test_A_Array for each new test_side1 sequence
    col5 = [0]* len(A_array[:,0])
    test_A_array = np.column_stack((A_col1, A_col2, A_col3, A_col4, col5))

    ### PIECES WITH ALL KNOWNS
    # add 1 to the row of the piece used for piece 1
    test=np.where(A_array == test_side1[i][0]+test_side1[i][1]+test_side1[i][4])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 3
    test=np.where(A_array == test_side1[i][9]+test_side1[i][10]+test_side1[i][13])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 5
    test=np.where(A_array == test_side1[i][18]+test_side1[i][19]+test_side1[i][22])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 7
    test=np.where(A_array == test_side1[i][27]+test_side1[i][28]+test_side1[i][31])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # PIECES 2, 4, 6 on the end of each seq
    # add 1 to the row of the piece used for piece 2
    test=np.where(A_array == test_side1[i][33]+test_side1[i][34]+test_side1[i][35])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 4
    test=np.where(A_array == test_side1[i][37]+test_side1[i][38]+test_side1[i][39])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 6
    test=np.where(A_array == test_side1[i][41]+test_side1[i][42]+test_side1[i][43])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # compare col4 to col3 by row, if col4<col3 the sequence has used a piece
    # more than once and is not a possibility
    count = 0 # count will count the number of rows where col4>col3
    for z in range(0,len(test_A_array[:, 0])): # z is the row number here
        if test_A_array[z][4]>test_A_array[z][3]:
                count=count+1
        else: count=count
    
    # if count > 0, pieces are used more than once and the sequence isn't possible
    # keep track of possible side 1 sequences
    if count==0:
        possible_side1.append(test_side1[i])
### end loop through all side 1 sequences
side1 = possible_side1


#=============================================================================
#SIDE 2
#=============================================================================

## look up each of the known pieces and add 1 in the last column of that row
## to indicate if the piece is used in the current sequence

## on side 2; piece 1 is now piece 7
# piece 2 is now piece 6
# piece 3 is now piece 8
# piece 4 is now piece 9
# piece 5 is now piece 10
# piece 6 is now piece 11
# piece 7 is now piece 12
# unknowns are piece 6/9/11

test_side2=[]
## start loop over all side 2 sequences
for k in range(0,len(side2)):
    ### start of loop for particular side sequence
    # determine options for unknown pieces
    piece6=[]
    for y in range(0, len(A)):
        if side2[k][6]+side2[k][7]==A[y][0]+A[y][1]:
            piece6.append(A[y][0]+A[y][1]+A[y][2])
            
    piece9=[]
    for y in range(0, len(A)):
        if side2[k][15]+side2[k][16]==A[y][0]+A[y][1]:
            piece9.append(A[y][0]+A[y][1]+A[y][2])
            
    piece11=[]
    for y in range(0, len(A)):
        if side2[k][24]+side2[k][25]==A[y][0]+A[y][1]:
            piece11.append(A[y][0]+A[y][1]+A[y][2])

    # permute the options, throw out duplicates
    unknowns=[","+ x + "-" + y + "-" + z for x in piece6 for y in piece9 for z in piece11]
    unknowns = list(set(unknowns)) # remove dups
    
    for j in range(0,len(unknowns)):
        test_side2.append(side2[k]+unknowns[j])
### end of loop that goes through each side's different options for unknowns

possible_side2=[] # reset 
## loop through this code to determine which pieces the test sequence
## (i.e. side 2 sequences + the specifications for pieces 6,9,11) uses
for i in range(0, len(test_side2)):
    #reset test_A_Array for each new test_side1 sequence
    col5 = [0]* len(A_array[:,0])
    test_A_array = np.column_stack((A_col1, A_col2, A_col3, A_col4, col5))

    ## PIECES WITH ALL KNOWNS
    # add 1 to the row of the piece used for piece 7
    test=np.where(A_array == test_side2[i][0]+test_side2[i][1]+test_side2[i][4])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 8
    test=np.where(A_array == test_side2[i][9]+test_side2[i][10]+test_side2[i][13])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 10
    test=np.where(A_array == test_side2[i][18]+test_side2[i][19]+test_side2[i][22])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 12
    test=np.where(A_array == test_side2[i][27]+test_side2[i][28]+test_side2[i][31])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    ### PIECES WITH UNKNOWNS
    # add 1 to the row of the piece used for piece 6
    test=np.where(A_array == test_side2[i][33]+test_side2[i][34]+test_side2[i][35])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 9
    test=np.where(A_array == test_side2[i][37]+test_side2[i][38]+test_side2[i][39])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 11
    test=np.where(A_array == test_side2[i][41]+test_side2[i][42]+test_side2[i][43])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # compare col4 to col3 by row, if col4<col3 the sequence has used a piece
    # more than once and is not a possibility
    count = 0 # count will count the number of rows where col4>col3
    for z in range(0,len(test_A_array[:, 0])): # z is the row number here
        if test_A_array[z][4]>test_A_array[z][3]:
                count=count+1
        else: count=count
    
    # if count > 0, pieces are used more than once and the sequence isn't possible
    # keep track of possible side 1 sequences
    if count==0:
        possible_side2.append(test_side2[i])
### end loop through all side 1 sequences
side2 = possible_side2

#=============================================================================

#=============================================================================
#SIDE 3 
#=============================================================================
## look up each of the known pieces and add 1 in the last column of that row
## to indicate if the piece is used in the current sequence

## on side 3; piece 7 is now piece 12
# piece 6 is now piece 11
# piece 8 is now piece 13
# piece 9 is now piece 14
# piece 10 is now piece 15
# piece 11 is now piece 2
# piece 12 is now piece 1
# unknowns are piece 11/14/2

test_side3=[]
## start loop over all side 3 sequences
for k in range(0,len(side3)):
    ### start of loop for particular side sequence
    # determine options for unknown pieces
    piece11=[]
    for y in range(0, len(A)):
        if side3[k][6]+side3[k][7]==A[y][0]+A[y][1]:
            piece11.append(A[y][0]+A[y][1]+A[y][2])
            
    piece14=[]
    for y in range(0, len(A)):
        if side3[k][15]+side3[k][16]==A[y][0]+A[y][1]:
            piece14.append(A[y][0]+A[y][1]+A[y][2])
            
    piece2=[]
    for y in range(0, len(A)):
        if side3[k][24]+side3[k][25]==A[y][0]+A[y][1]:
            piece2.append(A[y][0]+A[y][1]+A[y][2])

    # permute the options, throw out duplicates
    unknowns=[","+ x + "-" + y + "-" + z for x in piece11 for y in piece14 for z in piece2]
    unknowns = list(set(unknowns)) # remove dups
    
    for j in range(0,len(unknowns)):
        test_side3.append(side3[k]+unknowns[j])
### end of loop that goes through each side's different options for unknowns
## look up each of the known pieces and add 1 in the last column of that row
## to indicate if the piece is used in the current sequence
        
possible_side3=[] # reset 
## loop through this code to determine which pieces the test sequence
## (i.e. side 2 sequences + the specifications for pieces 6,9,11) uses
for i in range(0, len(test_side3)):
    #reset test_A_Array for each new test_side1 sequence
    col5 = [0]* len(A_array[:,0])
    test_A_array = np.column_stack((A_col1, A_col2, A_col3, A_col4, col5))

    ## PIECES WITH ALL KNOWNS
    # add 1 to the row of the piece used for piece 12
    test=np.where(A_array == test_side3[i][0]+test_side3[i][1]+test_side3[i][4])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 13
    test=np.where(A_array == test_side3[i][9]+test_side3[i][10]+test_side3[i][13])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 15
    test=np.where(A_array == test_side3[i][18]+test_side3[i][19]+test_side3[i][22])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 1
    test=np.where(A_array == test_side3[i][27]+test_side3[i][28]+test_side3[i][31])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    ### PIECES WITH UNKNOWNS
    # add 1 to the row of the piece used for piece 11
    test=np.where(A_array == test_side3[i][33]+test_side3[i][34]+test_side3[i][35])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 14
    test=np.where(A_array == test_side3[i][37]+test_side3[i][38]+test_side3[i][39])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 2
    test=np.where(A_array == test_side3[i][41]+test_side3[i][42]+test_side3[i][43])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # compare col4 to col3 by row, if col4<col3 the sequence has used a piece
    # more than once and is not a possibility
    count = 0 # count will count the number of rows where col4>col3
    for z in range(0,len(test_A_array[:, 0])): # z is the row number here
        if test_A_array[z][4]>test_A_array[z][3]:
                count=count+1
        else: count=count
    
    # if count > 0, pieces are used more than once and the sequence isn't possible
    # keep track of possible side 3 sequences
    if count==0:
        possible_side3.append(test_side3[i])
### end loop through all side 3 sequences
side3 = possible_side3   
#=============================================================================


################################# ALL SIDES #################################

# permute the combos of the sides
seq=[x +"--"+ y + "--" + z for x in side1 for y in side2 for z in side3]
#print(c)
len(seq)

seq = list(set(seq))

#=============================================================================
# side1, piece2-piece4-piece6
# side2, piece6-piece9-piece11
# side3, piece11-piece14-piece2

# implement piece 6 restrictions
# since the 3-letter piece will likely be in different orientations depending
# on the side perspective, better to do this by seeing if the row index matches
     
possible_seqs = []
for x in range(0,len(seq)):
        if int((np.where(A_array == seq[x][41]+seq[x][42]+seq[x][43]))[0])==\
            int((np.where(A_array == seq[x][79]+seq[x][80]+seq[x][81]))[0]):
            possible_seqs.append(seq[x])   
            possible_seqs = list(set(possible_seqs)) # remove dups
seq = possible_seqs

# implement piece 11 restrictions
possible_seqs = []
for x in range(0,len(seq)):
        if int((np.where(A_array == seq[x][87]+seq[x][88]+seq[x][89]))[0])==\
            int((np.where(A_array == seq[x][125]+seq[x][126]+seq[x][127]))[0]):
            possible_seqs.append(seq[x])   
            possible_seqs = list(set(possible_seqs)) # remove dups
seq = possible_seqs
            
# implement piece 2 restrictions
possible_seqs = []
for x in range(0,len(seq)):
        if int((np.where(A_array == seq[x][33]+seq[x][34]+seq[x][35]))[0])==\
            int((np.where(A_array == seq[x][133]+seq[x][134]+seq[x][135]))[0]):
            possible_seqs.append(seq[x])   
            possible_seqs = list(set(possible_seqs)) # remove dups
seq = possible_seqs
            

# implement corner piece restrictions 
# end of side 1 and beginning of side 2 must match
possible_seqs = []
for x in range(0,len(seq)):
        if int((np.where(A_array == seq[x][27]+seq[x][28]+seq[x][31]))[0])==\
            int((np.where(A_array == seq[x][46]+seq[x][47]+seq[x][50]))[0]):
            possible_seqs.append(seq[x])   
            possible_seqs = list(set(possible_seqs)) # remove dups
seq = possible_seqs

# end of side 2 and beginning of side 3 must match
possible_seqs = []
for x in range(0,len(seq)):
        if int((np.where(A_array == seq[x][73]+seq[x][74]+seq[x][77]))[0])==\
            int((np.where(A_array == seq[x][92]+seq[x][93]+seq[x][96]))[0]):
            possible_seqs.append(seq[x])   
            possible_seqs = list(set(possible_seqs)) # remove dups
seq = possible_seqs

# end of side 3 and beginning of side 1 must match
possible_seqs = []
for x in range(0,len(seq)):
        if int((np.where(A_array == seq[x][119]+seq[x][120]+seq[x][123]))[0])==\
            int((np.where(A_array == seq[x][0]+seq[x][1]+seq[x][4]))[0]):
            possible_seqs.append(seq[x])   
            possible_seqs = list(set(possible_seqs)) # remove dups
seq = possible_seqs

## determine middle piece
## see if middle piece exists in A
## finally count all pieces like before, using the whole sequence and determine
## which sequence uses only 1 of each piece.

## loop through this code to determine which pieces the sequence uses
possible_seq=[]

for i in range(0, len(seq)):
    col5 = [0]* len(A_array[:,0])
    test_A_array = np.column_stack((A_col1, A_col2, A_col3, A_col4, col5))
    
    # add 1 to the row of the piece used for piece 1
    test=np.where(A_array == seq[i][0]+seq[i][1]+seq[i][4])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 2
    test=np.where(A_array == seq[i][33]+seq[i][34]+seq[i][35])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 3
    test=np.where(A_array == seq[i][9]+seq[i][10]+seq[i][13])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 4
    test=np.where(A_array == seq[i][37]+seq[i][38]+seq[i][39])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 5
    test=np.where(A_array == seq[i][18]+seq[i][19]+seq[i][22])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 6
    test=np.where(A_array == seq[i][41]+seq[i][42]+seq[i][43])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 7
    test=np.where(A_array == seq[i][46]+seq[i][47]+seq[i][50])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 8
    test=np.where(A_array == seq[i][55]+seq[i][56]+seq[i][59])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1

    # add 1 to the row of the piece used for piece 9
    test=np.where(A_array == seq[i][83]+seq[i][84]+seq[i][85])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 10
    test=np.where(A_array == seq[i][64]+seq[i][65]+seq[i][68])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 11
    test=np.where(A_array == seq[i][87]+seq[i][88]+seq[i][89])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 12 
    test=np.where(A_array == seq[i][73]+seq[i][74]+seq[i][77])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 13
    test=np.where(A_array == seq[i][101]+seq[i][102]+seq[i][105])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 14
    test=np.where(A_array == seq[i][129]+seq[i][130]+seq[i][131])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 15
    test=np.where(A_array == seq[i][110]+seq[i][111]+seq[i][114])
    test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
    
    # add 1 to the row of the piece used for piece 16 (middle piece)
    #test=np.where(A_array == seq[i][41]+seq[i][42]+seq[i][43])
    #test_A_array[int(test[0])][4]=int(test_A_array[int(test[0])][4])+1
  
    # compare col3 to col4 by row, if col4<col3 the sequence has used a piece
    # more than once and is not a possibility
    count = 0 # count will count the number of rows where col4>col3
    for z in range(0,len(test_A_array[:, 0])): # z is the row number here
        if test_A_array[z][4]>test_A_array[z][3]:
                count=count+1
        else: count=count
    
    # if count > 0, pieces are used more than once and the sequence isn't possible
    # keep track of possible side 2 sequences
    if count==0:
        possible_seq.append(seq[i])
    if count>15:
        print("Error")
    print(count)
### end of loop

possible_seq

split_str= possible_seq[0].split("--")

split_str[0]
split_str[1]
split_str[2]

###double checking
# take a sequence and spit out the individual pieces 1-7
x=0
if x==0:
    print(split_str[x])
    print(split_str[x][0]+split_str[x][1]+split_str[x][4])
    print(split_str[x][33]+split_str[x][34]+split_str[x][35])
    print(split_str[x][9]+split_str[x][10]+split_str[x][13])
    print(split_str[x][37]+split_str[x][38]+split_str[x][39])
    print(split_str[x][18]+split_str[x][19]+split_str[x][22])
    print(split_str[x][41]+split_str[x][42]+split_str[x][43])
    print(split_str[x][27]+split_str[x][28]+split_str[x][31])

x=1
if x==1:
    print(split_str[x])
    print(split_str[x][0]+split_str[x][1]+split_str[x][4])
    print(split_str[x][33]+split_str[x][34]+split_str[x][35])
    print(split_str[x][9]+split_str[x][10]+split_str[x][13])
    print(split_str[x][37]+split_str[x][38]+split_str[x][39])
    print(split_str[x][18]+split_str[x][19]+split_str[x][22])
    print(split_str[x][41]+split_str[x][42]+split_str[x][43])
    print(split_str[x][27]+split_str[x][28]+split_str[x][31])

x=2
if x==2:
    print(split_str[x])
    print(split_str[x][0]+split_str[x][1]+split_str[x][4])
    print(split_str[x][33]+split_str[x][34]+split_str[x][35])
    print(split_str[x][9]+split_str[x][10]+split_str[x][13])
    print(split_str[x][37]+split_str[x][38]+split_str[x][39])
    print(split_str[x][18]+split_str[x][19]+split_str[x][22])
    print(split_str[x][41]+split_str[x][42]+split_str[x][43])
    print(split_str[x][27]+split_str[x][28]+split_str[x][31])

