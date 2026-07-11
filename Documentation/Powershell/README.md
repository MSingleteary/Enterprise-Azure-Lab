# ==========================================
# Resume Technologies
# Automated Employee Onboarding
# Step 2: CSV Validation
# ==========================================

Import-Module ActiveDirectory

$CsvPath = "C:\IAMReports\NewHires.csv"

if (-not (Test-Path $CsvPath)) {
    Write-Error "CSV file not found: $CsvPath"
    exit 1
}

$Employees = Import-Csv $CsvPath

if (-not $Employees -or $Employees.Count -eq 0) {
    Write-Error "The CSV contains no employee records."
    exit 1
}

$RequiredColumns = @(
    "FirstName",
    "LastName",
    "Department",
    "Title",
    "EmployeeID",
    "Username",
    "Email"
)

$ActualColumns = $Employees[0].PSObject.Properties.Name

$MissingColumns = $RequiredColumns | Where-Object {
    $_ -notin $ActualColumns
}

if ($MissingColumns.Count -gt 0) {
    Write-Error "Missing required CSV columns: $($MissingColumns -join ', ')"
    exit 1
}

Write-Host ""
Write-Host "CSV validation passed." -ForegroundColor Green
Write-Host "Employee records found: $($Employees.Count)"
Write-Host ""

$Employees |
Format-Table FirstName, LastName, Department, Title, Username
