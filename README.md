import 'dart:io';

class Student {
  String name;
  double ca;
  double exam;
  double project;
  double total;
  String grade;

  Student(this.name, this.ca, this.exam, this.project) {
    total = ca + exam + project;
    grade = _calculateGrade(total);
  }

  String _calculateGrade(double total) {
    if (total >= 80) return 'A';
    if (total >= 75) return 'B+';
    if (total >= 70) return 'B';
    if (total >= 65) return 'C+';
    if (total >= 60) return 'C';
    if (total >= 55) return 'D+';
    if (total >= 50) return 'D';
    if (total >= 45) return 'E';
    return 'F';
  }
}

void main() {
  print("Dr. Asante's Grade Calculator");
  print("----------------------------\n");

  List<Student> students = [];
  
  stdout.write("Enter number of students: ");
  int numStudents = int.parse(stdin.readLineSync()!);

  for (int i = 0; i < numStudents; i++) {
    print("\nEnter details for Student ${i + 1}:");
    
    stdout.write("Student Name: ");
    String name = stdin.readLineSync()!;
    
    stdout.write("Continuous Assessment Score (0-30): ");
    double ca = double.parse(stdin.readLineSync()!);
    
    stdout.write("Exam Score (0-50): ");
    double exam = double.parse(stdin.readLineSync()!);
    
    stdout.write("Project Score (0-20): ");
    double project = double.parse(stdin.readLineSync()!);

    // Validate inputs
    if (ca < 0 || ca > 30 || exam < 0 || exam > 50 || project < 0 || project > 20) {
      print("\nError: Invalid scores entered!");
      print("Please ensure:");
      print("Continuous Assessment: 0-30");
      print("Exam Score: 0-50");
      print("Project Score: 0-20");
      return;
    }

    students.add(Student(name, ca, exam, project));
  }

  // Display results
  print("\n\nGRADING RESULTS");
  print("-----------------------------------------------");
  print("Name                CA    Exam  Project Total Grade");
  print("-----------------------------------------------");
  
  for (var student in students) {
    print("${student.name.padRight(20)} ${student.ca.toStringAsFixed(1).padLeft(5)} "
        "${student.exam.toStringAsFixed(1).padLeft(6)} ${student.project.toStringAsFixed(1).padLeft(8)} "
        "${student.total.toStringAsFixed(1).padLeft(5)} ${student.grade.padLeft(6)}");
  }

  // Option to save to file
  stdout.write("\nWould you like to save these results to a file? (y/n): ");
  String save = stdin.readLineSync()!.toLowerCase();
  
  if (save == 'y') {
    stdout.write("Enter filename (e.g., grades.txt): ");
    String filename = stdin.readLineSync()!;
    
    File file = File(filename);
    file.writeAsStringSync("GRADING RESULTS\n");
    file.writeAsStringSync("-----------------------------------------------\n", mode: FileMode.append);
    file.writeAsStringSync("Name                CA    Exam  Project Total Grade\n", mode: FileMode.append);
    file.writeAsStringSync("-----------------------------------------------\n", mode: FileMode.append);
    
    for (var student in students) {
      file.writeAsStringSync(
        "${student.name.padRight(20)} ${student.ca.toStringAsFixed(1).padLeft(5)} "
        "${student.exam.toStringAsFixed(1).padLeft(6)} ${student.project.toStringAsFixed(1).padLeft(8)} "
        "${student.total.toStringAsFixed(1).padLeft(5)} ${student.grade.padLeft(6)}\n",
        mode: FileMode.append
      );
    }
    
    print("Results saved to $filename");
  }
}
