from flask import Flask, render_template, request, redirect, url_for
import pandas as pd

app = Flask(__name__)

# Initialize DataFrame to store student grades
columns = ['Student ID', 'Name', 'Class']
subjects = ['Math', 'Science', 'English']  # Add more subjects as needed
columns.extend(subjects)
grades_df = pd.DataFrame(columns=columns)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/add_grade', methods=['GET', 'POST'])
def add_grade():
    if request.method == 'POST':
        student_id = request.form['student_id']
        name = request.form['name']
        student_class = request.form['class']
        grades = [int(request.form[subject]) for subject in subjects]
        
        # Append new row to the DataFrame
        global grades_df
        grades_df = grades_df.append(pd.Series([student_id, name, student_class] + grades, index=grades_df.columns), ignore_index=True)
        return redirect(url_for('index'))
    return render_template('add_grade.html', subjects=subjects)

# Add routes for average grade calculation, report generation, and student records management

if __name__ == '__main__':
    app.run(debug=True)
# Studentgradetracker
