Ingres dialect for SQLAlchemy https://github.com/sqlalchemy/sqlalchemy.

Originally developed to work against SQLAlchemy 0.6 and Ingres 9.2

Current work-in-progress with:

  * SQLAlchemy 1.3.16 and 1.4.0b1
  * Ingres 11.1 and Vector 5.1 - via ODBC


## Development instructions

Right now this is for dev purposes so install SQLAlchemy as per normal, for example:

    pip install sqlalchemy

or for dev testing and modifying/running tests:

    git clone https://github.com/sqlalchemy/sqlalchemy.git
    cd sqlalchemy
    pip install -e .
    #python -m pip install -e .

Ingres dialect tested with pyodbc and pypyodbc (pypyodbc useful for debugging, to see what SQLAlchemy is calling with):

    pip install pyodbc

Download Ingres dialect for SQLAlchemy:

    git clone https://github.com/clach04/ingres_sa_dialect.git

Setup for dev use:

    cd ingres_sa_dialect
    pip install -e .

Demo/Test:

Ensure Ingres ODBC driver is available, Ingres sqlalchemy defaults to using "Ingres" ODBC driver, ensure this is the same bit-age as the Python interpreter. For example, for 64-bit Python, ensure ODBC Driver called "Ingres" is also 64-bit.

ODBC Driver name can be overridden via environment variable `SQLALCHEMY_INGRES_ODBC_DRIVER_NAME`, for example:

Windows 64-bit:

    set SQLALCHEMY_INGRES_ODBC_DRIVER_NAME=Ingres CS

Windows 32-bit:

    set SQLALCHEMY_INGRES_ODBC_DRIVER_NAME=Ingres CR

Under Linux/Unix check ODBC settings and if using UnixODBC, check how wide-char support was built, recommendation for out-of-box Linux distributions:

    # Most Linux distros build UnixODBC with non-default build options
    export II_ODBC_WCHAR_SIZE=2

    # set variables for ODBC config
    export ODBCSYSINI=$II_SYSTEM/ingres/files
    export ODBCINI=$II_SYSTEM/ingres/files/odbc.ini

Quick python test:

    import sys
    import sqlalchemy
    
    print('Python %s on %s' % (sys.version, sys.platform))
    print(sqlalchemy.__version__)
    con_str = 'ingres:///demodb'  # local demodb
    #con_str = 'ingres://dbuser:PASSWORD@HOSTNAME:27832/db'  # remote database called "db"
    engine = sqlalchemy.create_engine(con_str)
    connection = engine.connect()
    query = 'SELECT * FROM iidbconstants'
    for row in connection.execute(sqlalchemy.text(query)):
        print(row)
