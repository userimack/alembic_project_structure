# alembic_project_structure
Alembic project strucutre and changes to use autogenerate options

Project Structure

    alembic_project_structure [PROJECT ROOT]
    ├── __init__.py
    ├── alembic.ini
    └── database
        ├── __init__.py
        ├── migrations
        │   ├── README
        │   ├── env.py
        │   ├── script.py.mako
        │   └── versions
        │       ├── 30b29ae18809_string_fix.py
        │       ├── 3d59d85413f2_string_fix2.py
        │       ├── 46145785bf85_added_account_table.py
        └── models.py [MODELS FOR WHICH WE WANT TO AUTOGENERATE MIGRATIONS]

Changes done has been described below

# alembic.ini

    # path to migration scripts
    script_location = database/migrations




# database/migrations/env.py
Add the following lines so that import works

    import os, sys
    sys.path.append(os.getcwd())

    from database import models
    target_metadata = models.Base.metadata

    ...
    ...

    def run_migrations_online():
        """Run migrations in 'online' mode.

        In this scenario we need to create an Engine
        and associate a connection with the context.

        """
        connectable = engine_from_config(
            config.get_section(config.config_ini_section),
            prefix='sqlalchemy.',
            poolclass=pool.NullPool)

        with connectable.connect() as connection:
            context.configure(
                connection=connection,
                target_metadata=target_metadata,
                *compare_type=True,*
            )

            with context.begin_transaction():
                context.run_migrations()



