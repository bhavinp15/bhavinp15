## Hi there 👋

<!--
**bhavinp15/bhavinp15** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

import tkinter as tk
from tkinter import ttk
import math

class Calculator:
    def __init__(self, root):
        self.root = root
        self.root.title("Calculator")
        self.root.geometry("400x600")
        self.root.resizable(False, False)
        self.root.configure(bg='#2b2b2b')
        
        # Variables
        self.current_input = ""
        self.total = 0
        self.operator = ""
        self.result_var = tk.StringVar()
        self.result_var.set("0")
        
        # Create UI
        self.create_display()
        self.create_buttons()
        
        # Bind keyboard events
        self.root.bind('<Key>', self.key_press)
        self.root.focus_set()
    
    def create_display(self):
        """Create the display area"""
        display_frame = tk.Frame(self.root, bg='#2b2b2b', padx=10, pady=10)
        display_frame.pack(fill=tk.X)
        
        # Result display
        self.display = tk.Entry(
            display_frame,
            textvariable=self.result_var,
            font=('Arial', 32, 'bold'),
            justify='right',
            state='readonly',
            bg='#1a1a1a',
            fg='white',
            bd=0,
            relief=tk.FLAT,
            readonlybackground='#1a1a1a'
        )
        self.display.pack(fill=tk.X, ipady=20)
    
    def create_buttons(self):
        """Create calculator buttons"""
        buttons_frame = tk.Frame(self.root, bg='#2b2b2b', padx=10, pady=10)
        buttons_frame.pack(fill=tk.BOTH, expand=True)
        
        # Button configurations: (text, row, col, colspan, color_type)
        buttons = [
            ('C', 0, 0, 1, 'function'),
            ('CE', 0, 1, 1, 'function'),
            ('⌫', 0, 2, 1, 'function'),
            ('÷', 0, 3, 1, 'operator'),
            
            ('7', 1, 0, 1, 'number'),
            ('8', 1, 1, 1, 'number'),
            ('9', 1, 2, 1, 'number'),
            ('×', 1, 3, 1, 'operator'),
            
            ('4', 2, 0, 1, 'number'),
            ('5', 2, 1, 1, 'number'),
            ('6', 2, 2, 1, 'number'),
            ('-', 2, 3, 1, 'operator'),
            
            ('1', 3, 0, 1, 'number'),
            ('2', 3, 1, 1, 'number'),
            ('3', 3, 2, 1, 'number'),
            ('+', 3, 3, 1, 'operator'),
            
            ('±', 4, 0, 1, 'function'),
            ('0', 4, 1, 1, 'number'),
            ('.', 4, 2, 1, 'number'),
            ('=', 4, 3, 1, 'equals'),
            
            # Scientific functions (additional row)
            ('√', 5, 0, 1, 'function'),
            ('x²', 5, 1, 1, 'function'),
            ('1/x', 5, 2, 1, 'function'),
            ('%', 5, 3, 1, 'function'),
        ]
        
        # Configure grid weights
        for i in range(6):
            buttons_frame.grid_rowconfigure(i, weight=1)
        for i in range(4):
            buttons_frame.grid_columnconfigure(i, weight=1)
        
        # Button colors
        colors = {
            'number': {'bg': '#404040', 'fg': 'white', 'active_bg': '#505050'},
            'operator': {'bg': '#ff9500', 'fg': 'white', 'active_bg': '#ffb143'},
            'function': {'bg': '#a6a6a6', 'fg': 'black', 'active_bg': '#b8b8b8'},
            'equals': {'bg': '#ff9500', 'fg': 'white', 'active_bg': '#ffb143'}
        }
        
        # Create buttons
        for text, row, col, colspan, color_type in buttons:
            btn = tk.Button(
                buttons_frame,
                text=text,
                font=('Arial', 20, 'bold'),
                bg=colors[color_type]['bg'],
                fg=colors[color_type]['fg'],
                activebackground=colors[color_type]['active_bg'],
                activeforeground=colors[color_type]['fg'],
                bd=0,
                relief=tk.FLAT,
                command=lambda t=text: self.button_click(t)
            )
            btn.grid(row=row, column=col, columnspan=colspan, 
                    sticky='nsew', padx=2, pady=2)
    
    def button_click(self, char):
        """Handle button clicks"""
        if char in '0123456789.':
            self.input_number(char)
        elif char in '+-×÷':
            self.input_operator(char)
        elif char == '=':
            self.calculate()
        elif char == 'C':
            self.clear_all()
        elif char == 'CE':
            self.clear_entry()
        elif char == '⌫':
            self.backspace()
        elif char == '±':
            self.toggle_sign()
        elif char == '√':
            self.square_root()
        elif char == 'x²':
            self.square()
        elif char == '1/x':
            self.reciprocal()
        elif char == '%':
            self.percentage()
    
    def input_number(self, char):
        """Input a number or decimal point"""
        if char == '.' and '.' in self.current_input:
            return
        
        if self.current_input == "0" and char != '.':
            self.current_input = char
        elif self.current_input == "Error":
            self.current_input = char
        else:
            self.current_input += char
        
        self.update_display(self.current_input)
    
    def input_operator(self, op):
        """Input an operator"""
        if self.current_input == "":
            return
        
        if self.operator and self.current_input:
            self.calculate()
        
        self.total = float(self.current_input) if self.current_input else 0
        self.operator = op
        self.current_input = ""
    
    def calculate(self):
        """Perform calculation"""
        if not self.operator or not self.current_input:
            return
        
        try:
            current = float(self.current_input)
            
            if self.operator == '+':
                result = self.total + current
            elif self.operator == '-':
                result = self.total - current
            elif self.operator == '×':
                result = self.total * current
            elif self.operator == '÷':
                if current == 0:
                    raise ZeroDivisionError
                result = self.total / current
            
            # Format result
            if result == int(result):
                result = int(result)
            else:
                result = round(result, 10)
            
            self.current_input = str(result)
            self.update_display(self.current_input)
            self.operator = ""
            self.total = 0
            
        except ZeroDivisionError:
            self.current_input = "Error"
            self.update_display("Cannot divide by zero")
            self.operator = ""
            self.total = 0
        except Exception:
            self.current_input = "Error"
            self.update_display("Error")
            self.operator = ""
            self.total = 0
    
    def clear_all(self):
        """Clear everything"""
        self.current_input = ""
        self.total = 0
        self.operator = ""
        self.update_display("0")
    
    def clear_entry(self):
        """Clear current entry"""
        self.current_input = ""
        self.update_display("0")
    
    def backspace(self):
        """Remove last character"""
        if self.current_input and self.current_input != "Error":
            self.current_input = self.current_input[:-1]
            if not self.current_input:
                self.update_display("0")
            else:
                self.update_display(self.current_input)
    
    def toggle_sign(self):
        """Toggle positive/negative"""
        if self.current_input and self.current_input != "0" and self.current_input != "Error":
            if self.current_input.startswith('-'):
                self.current_input = self.current_input[1:]
            else:
                self.current_input = '-' + self.current_input
            self.update_display(self.current_input)
    
    def square_root(self):
        """Calculate square root"""
        if not self.current_input or self.current_input == "Error":
            return
        
        try:
            num = float(self.current_input)
            if num < 0:
                raise ValueError
            result = math.sqrt(num)
            if result == int(result):
                result = int(result)
            else:
                result = round(result, 10)
            self.current_input = str(result)
            self.update_display(self.current_input)
        except ValueError:
            self.current_input = "Error"
            self.update_display("Invalid input")
    
    def square(self):
        """Calculate square"""
        if not self.current_input or self.current_input == "Error":
            return
        
        try:
            num = float(self.current_input)
            result = num * num
            if result == int(result):
                result = int(result)
            else:
                result = round(result, 10)
            self.current_input = str(result)
            self.update_display(self.current_input)
        except ValueError:
            self.current_input = "Error"
            self.update_display("Error")
    
    def reciprocal(self):
        """Calculate reciprocal (1/x)"""
        if not self.current_input or self.current_input == "Error":
            return
        
        try:
            num = float(self.current_input)
            if num == 0:
                raise ZeroDivisionError
            result = 1 / num
            if result == int(result):
                result = int(result)
            else:
                result = round(result, 10)
            self.current_input = str(result)
            self.update_display(self.current_input)
        except ZeroDivisionError:
            self.current_input = "Error"
            self.update_display("Cannot divide by zero")
    
    def percentage(self):
        """Calculate percentage"""
        if not self.current_input or self.current_input == "Error":
            return
        
        try:
            num = float(self.current_input)
            result = num / 100
            if result == int(result):
                result = int(result)
            else:
                result = round(result, 10)
            self.current_input = str(result)
            self.update_display(self.current_input)
        except ValueError:
            self.current_input = "Error"
            self.update_display("Error")
    
    def key_press(self, event):
        """Handle keyboard input"""
        key = event.char
        
        if key in '0123456789.':
            self.input_number(key)
        elif key in '+-*/':
            # Map keyboard operators to calculator operators
            op_map = {'+': '+', '-': '-', '*': '×', '/': '÷'}
            self.input_operator(op_map[key])
        elif key in '\r=':  # Enter or equals
            self.calculate()
        elif event.keysym == 'BackSpace':
            self.backspace()
        elif event.keysym == 'Escape':
            self.clear_all()
        elif event.keysym == 'Delete':
            self.clear_entry()
    
    def update_display(self, value):
        """Update the display"""
        # Limit display length
        if len(str(value)) > 15:
            value = f"{float(value):.6e}"  # Scientific notation for very large numbers
        
        self.result_var.set(str(value))

def main():
    """Main function to run the calculator"""
    root = tk.Tk()
    calculator = Calculator(root)
    
    # Center the window
    root.update_idletasks()
    width = root.winfo_width()
    height = root.winfo_height()
    x = (root.winfo_screenwidth() // 2) - (width // 2)
    y = (root.winfo_screenheight() // 2) - (height // 2)
    root.geometry(f'{width}x{height}+{x}+{y}')
    
    root.mainloop()

if __name__ == "__main__":
    main()
