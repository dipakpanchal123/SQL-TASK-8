USE LibraryDB;
DELIMITER //
CREATE PROCEDURE GetBooksByAuthor(IN authorName VARCHAR(100))
BEGIN
    SELECT B.Title, B.Genre, B.PublishedYear
    FROM Books B
    JOIN Authors A ON B.AuthorID = A.AuthorID
    WHERE A.Name = authorName;
END //
DELIMITER ;
CALL GetBooksByAuthor('J.K. Rowling');
DELIMITER //
CREATE PROCEDURE AddNewMember(
    IN memberName VARCHAR(100),
    IN memberEmail VARCHAR(100),
    IN memberPhone VARCHAR(15),
    IN joinDate DATE
)
BEGIN
    INSERT INTO Members(Name, Email, Phone, MembershipDate)
    VALUES(memberName, memberEmail, memberPhone, joinDate);
END //
DELIMITER ;
CALL AddNewMember('Rahul Sharma', 'rahul@gmail.com', '9876501234', '2024-06-15');
DELIMITER //
CREATE FUNCTION GetBorrowCount(memberId INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE totalBorrows INT;
    SELECT COUNT(*) INTO totalBorrows
    FROM Borrow
    WHERE MemberID = memberId;
    RETURN totalBorrows;
END //
DELIMITER ;
SELECT GetBorrowCount(1) AS TotalBorrowedByMember1;
DELIMITER //
CREATE FUNCTION GetLateDays(borrowId INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE lateDays INT;
    SELECT DATEDIFF(ReturnDate, BorrowDate) INTO lateDays
    FROM Borrow
    WHERE BorrowID = borrowId;
    RETURN IFNULL(lateDays, 0);
END //
DELIMITER ;
SELECT GetLateDays(1) AS DaysTakenForBorrow1;
