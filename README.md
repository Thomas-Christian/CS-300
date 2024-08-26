In these projects, I developed a course planner application for ABCU's Computer Science program. The main challenge was to efficiently store, retrieve, and display course information, including prerequisites. I approached this problem by implementing and comparing different data structures: vectors, hash tables, and binary search trees. This process highlighted the importance of understanding data structures. Each had unique strengths and trade-offs in terms of runtime efficiency and memory usage. Vectors offered simplicity, hash tables provided faster lookups, and binary search trees maintained sorted order efficiently. One significant roadblock was ensuring data validation when reading the course file. I overcame this by implementing a thorough validation function. It checked for proper file formatting and the existence of prerequisites. This experience taught me the importance of error handling and input validation in creating reliable software. Working on these projects has expanded my approach to software design. It emphasized the need to consider both functionality and performance. I learned to analyze runtime complexity and make informed decisions about which data structure to use based on specific requirements. This has made me more thoughtful about the broader implications of design choices in software development. Furthermore, this project has evolved my coding practices. I now prioritize maintainability, readability, and adaptability. I've improved my code organization, using clear function names and comments to explain complex logic. The modular design I implemented has made the code more adaptable to potential future changes or expansions. In conclusion, these projects have enhanced my technical skills in implementing data structures and algorithms. They have also deepened my understanding of effective software design principles and practices.


function readAndValidateFile(filename)
    open file(filename)  								O(1)
    if file not opened successfully then  						O(1)
        print "Error: Unable to open file"  						O(1)
        return false  									O(1)
    
    create empty set validCourses  							O(1)
    
    while not end of file  								O(n) 
        read line from file  								O(1)
        split line into tokens using comma as delimiter  				O(n char/line) 
        if number of tokens < 2 then							O(1)
            print "Error: Invalid file format." 						O(1)
            return false									O(1)
        
        courseNumber = tokens[0]  							O(1)
        add courseNumber to validCourses  						O(1)

    reset file pointer to beginning  							O(1)
    
    while not end of file 								O(n)
        read line from file  								O(1)
        split line into tokens using comma as delimiter 					O(n char/line)
        for each prerequisite in tokens[2] to tokens[n]  					O(n prereqs)
            if prerequisite not in validCourses then  					O(1)
                print "Error: Prerequisite does not exist as a course."  			O(1)
                return false  								O(1)
    
    close file  										O(1)
    return true  										O(1)

Worst: O(n*(n char/line + n prereqs))


function createCourseVector(filename)
    create empty vector courses 							O(1)
    
    open file(filename)  								O(1)
    while not end of file  								O(n)
        read line from file 								O(1)
        split line into tokens using comma as delimiter 					O(n char/line)
        
        create new Course object  							O(1)
        set Course.courseNumber = tokens[0]  						O(1)
        set Course.name = tokens[1]  							O(1)
        for each prerequisite in tokens[2] to tokens[n]  					O(n prereqs)
            add prerequisite to Course.prerequisites  					O(1)
        
        add Course to courses vector  							O(1) 

    close file  										O(1)
    return courses  									O(1)

Worst: O(n*(n char/line + n prereqs))
Advantages:
Simple implementation
Fast insertion (at the end)
Disadvantages:
Slow insertion/deletion in the middle 
Slow search time 


function createCourseHashTable(filename)
    create empty hash table courses  							O(1)
    
    open file(filename) 									O(1)
    while not end of file  								O(n)
        read line from file 								O(1)
        split line into tokens using comma as delimiter 					O(n char/line)
        
        create new Course object  							O(1)
        set Course.courseNumber = tokens[0] 						O(1)
        set Course.name = tokens[1] 							O(1)
        for each prerequisite in tokens[2] to tokens[n]  					O(n prereqs)
            add prerequisite to Course.prerequisites  					O(1)
        
        insert Course into courses hash table using courseNumber as key  		O(1)
    close file  										O(1)
    return courses  									O(1)

Worst: O(n*(n char/line + n prereqs))
Advantages:
Fast average-case insertion, deletion, and search
Efficient for direct access by course number
Disadvantages:
Potential for collisions
Not necessarily ordered


function createCourseBST(filename)
    create new BinarySearchTree courses  						O(1)
    
    open file(filename)  								O(1)
    while not end of file  								O(n)
        read line from file 								O(1)
        split line into tokens using comma as delimiter 					O(n char/line)
        
        create new Course object 							O(1)
        set Course.courseNumber = tokens[0]  						O(1)
        set Course.name = tokens[1]  							O(1)
        for each prerequisite in tokens[2] to tokens[n]  					O(n prereqs)
            add prerequisite to Course.prerequisites  					O(1)
        
        courses.insert(Course)  								O(log n)

    close file  										O(1)
    return courses 									O(1)

Worst: O(n * (n char/line + n prereqs + log n))
Advantages:
Efficient search, insertion, and deletion
Maintains sorted order
Disadvantages:
Can degenerate in performance if unbalanced
More complex implementation


void printCourses(vector<Node> courses) {
    // vector for sort
    vector<Course> coursesSorted;

    for (const auto& node : courses) {
        if (node.key != UINT_MAX) {
            Node* current = const_cast<Node*>(&node);  // start at the first node

            // traverse list and collect courses
            while (current != nullptr) {
                coursesSorted.push_back(current->course);
                current = current->next;
            }
        }
    }

    // sort courses alphanumerically by course number
    sort(coursesSorted.begin(), coursesSorted.end(), [](const Course& a, const Course& b) {
        return a.courseNumber < b.courseNumber;
    });

    // print info
    for (int i = 0; i < coursesSorted.size(); i++)
    {
        // remove extra endl for last course
        if (i != coursesSorted.size() - 1) {
            cout << "Course Number: " << coursesSorted.at(i).courseNumber << endl;
            cout << "Course Title: " << coursesSorted.at(i).courseTitle << endl;
            if (!coursesSorted.at(i).prerequisites.empty()) {
                cout << "Prerequisites: ";
                for (const auto& pre : coursesSorted.at(i).prerequisites) {
                    cout << pre << " ";
                }
                cout << endl;
            }
            else {
                cout << "Prerequisites: None" << endl;
            }
            cout << endl;
        }
        else {
            cout << "Course Number: " << coursesSorted.at(i).courseNumber << endl;
            cout << "Course Title: " << coursesSorted.at(i).courseTitle << endl;
            if (!coursesSorted.at(i).prerequisites.empty()) {
                cout << "Prerequisites: ";
                for (const auto& pre : coursesSorted.at(i).prerequisites) {
                    cout << pre << " ";
                }
                cout << endl;
            }
            else {
                cout << "Prerequisites: None" << endl;
            }
        }
    }
}
