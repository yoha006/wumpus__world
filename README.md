<h1>ExpNo 8 : Solve Cryptarithmetic Problem,a CSP(Constraint Satisfaction Problem) using Python</h1> 
<h3>Name: P.YOHALAKSHMI     </h3>
<h3>Register Number: 212224060314  </h3>
<H3>Aim:</H3>
<p>
    To solve Cryptarithmetic Problem,a CSP(Constraint Satisfaction Problem) using Python
</p>
<h3>Procedure:</h3>
Input and Output
<br>Input:
This algorithm will take three words.
<br> B A S E<br>
    B A L L<br>
           ----------<br>
           G A M E S<br>

Output:
It will show which letter holds which number from 0 – 9.
For this case it is like this.

              B A S E                         2 4 6 1
              B A L L                         2 4 5 5
             ---------                       ---------
            G A M E S                       0 4 9 1 6
Algorithm
For this problem, we will define a node, which contains a letter and its corresponding values.<br>

isValid(nodeList, count, word1, word2, word3)<br>

Input − A list of nodes, the number of elements in the node list and three words.<br>

Output − True if the sum of the value for word1 and word2 is same as word3 value.<br>

Begin<br>
   m := 1<br>
   for each letter i from right to left of word1, do<br>
      ch := word1[i]<br>
      for all elements j in the nodeList, do<br>
         if nodeList[j].letter = ch, then<br>
            break<br>
      done<br>
      val1 := val1 + (m * nodeList[j].value)<br>
      m := m * 10<br>
   done<br>

   m := 1<br>
   for each letter i from right to left of word2, do<br>
      ch := word2[i]<br>
      for all elements j in the nodeList, do<br>
         if nodeList[j].letter = ch, then<br>
            break<br>
      done<br>

      val2 := val2 + (m * nodeList[j].value)
      m := m * 10
   done<br>

   m := 1<br>
   for each letter i from right to left of word3, do<br>
      ch := word3[i]<br>
      for all elements j in the nodeList, do<br>
         if nodeList[j].letter = ch, then<br>
            break<br>
      done<br>

      val3 := val3 + (m * nodeList[j].value)
      m := m * 10
   done<br>

   if val3 = (val1 + val2), then<br>
      return true<br>
   return false<br>
End<br>
<hr>
<h2>Sample Input and Output:</h2>
SEND = 9567<br>
MORE = 1085<br>
<hr>
MONEY = 10652<br>

# Program:
    import itertools

    def solve_cryptarithmetic(addends, result, find_all=False, verbose=False):
    words = addends + [result]
    
    # Extract all unique letters
    letters = []
    for w in words:
        for ch in w:
            if ch not in letters:
                letters.append(ch)

    if len(letters) > 10:
        raise ValueError("Too many letters. Max 10 allowed.")

    # leading letters cannot be zero
    leading = {w[0] for w in words}

    def word_value(word, mapping):
        val = 0
        for c in word:
            val = val * 10 + mapping[c]
        return val

    solutions = []

    for perm in itertools.permutations(range(10), len(letters)):
        mapping = dict(zip(letters, perm))

        # leading zero check
        if any(mapping[ch] == 0 for ch in leading):
            continue

        # compute sum of addends
        add_sum = sum(word_value(w, mapping) for w in addends)
        result_val = word_value(result, mapping)

        if add_sum == result_val:
            solutions.append(mapping.copy())
            if not find_all:
                return mapping

    return solutions if find_all else (solutions[0] if solutions else None)


    # -------------------------------
    # NEW EXAMPLE
    # BASE + BALL = GAMES
    # -------------------------------
    if __name__ == "__main__":
    addends = ["BASE", "BALL"]
    result = "GAMES"

    solution = solve_cryptarithmetic(addends, result, verbose=True)

    if solution:
        print("\nSolution Found:")
        for k in sorted(solution):
            print(f"{k} -> {solution[k]}")

        def num(word):
            return int("".join(str(solution[ch]) for ch in word))

        print(f"\nCheck:")
        print(f"{addends[0]} ({num(addends[0])})")
        print(f"+ {addends[1]} ({num(addends[1])})")
        print(f"= {result} ({num(result)})")

    else:
        print("No solution found.")
<hr>
<h2>Output:</h2>
<img width="668" height="316" alt="image" src="https://github.com/user-attachments/assets/22e10a07-802f-4394-8829-5557725e9a64" />


<hr>
<h2>Result:</h2>
<p> Thus a Cryptarithmetic Problem was solved using Python successfully</p>
