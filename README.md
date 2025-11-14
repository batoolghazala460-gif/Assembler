# Assembler
Pseudo Code: 
Step 1: Program Start & Input Handling 
1. Start the program. 
2. Read three arguments from the command line: 
• Source file (assembly program). 
• Destination file (output machine code). 
• Output format (-h for hexadecimal, -b for binary). 
3. Open the source file for reading and destination file for writing. 
4. If files cannot be opened, print an error and exit. 
Step 2: First Pass (Label Collection) 
1. Initialize an instruction address counter starting from 0. 
2. Read the source file line by line. 
3. For each line: 
• If the line contains a label (detected by :), extract the label name and store it with the 
current address in a label table. 
• Otherwise, treat the line as an instruction and increase the address counter by 4 (since each 
instruction is 32 bits). 
4. When the file ends, rewind to the beginning for the second pass. 
Step 3: Second Pass (Instruction Translation) 
1. Reset the instruction address counter to 0. 
2. Read the source file again line by line. 
3. For each line: 
• If the line is a label or a comment, skip it. 
• If the line is empty or whitespace, skip it. 
• Otherwise, identify the opcode (e.g., add, lw, beq, etc.). 
• Based on instruction type (R, I, S, B, U, J), call the appropriate assembler function. 
Step 4: Instruction Parsing 
1. R-Type (e.g., add rd, rs1, rs2) 
• Extract destination and source registers. 
• Map register names (e.g., x5, a0) to their numeric codes. 
• Construct machine code using funct3, funct7, and opcode fields. 
2. I-Type (e.g., lw rd, imm(rs1) or addi rd, rs1, imm) 
• Extract destination, source register, and immediate value. 
• Perform sign extension for immediate values. 
• Assemble fields into 32-bit machine code. 
3. S-Type (e.g., sw rs2, imm(rs1)) 
• Extract two source registers and immediate value. 
• Split the immediate into proper bit fields before encoding. 
4. B-Type (e.g., beq rs1, rs2, label) 
• Extract registers and branch target label. 
• Resolve the label address from the table. 
• Compute PC-relative offset and encode into immediate fields. 
5. U-Type (e.g., lui rd, imm) 
• Extract destination register and immediate. 
• Shift and encode into upper 20 bits of machine code. 
6. J-Type (e.g., jal rd, label) 
• Extract destination register and label. 
• Resolve the label address. 
• Calculate offset relative to current address and encode in the J-format. 
Step 5: Output Generation 
1. Once machine code for an instruction is generated: 
• If output type is hexadecimal, write code as 0xXXXXXXXX. 
• If output type is binary, write 32 bits as a binary string. 
2. Append each result to the destination file line by line. 
3. Increase instruction address by 4. 
Step 6: Program End 
1. After all instructions are processed, close both source and destination files. 
2. Exit successfully. 
