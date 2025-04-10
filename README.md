# auto-test
auto-test
<changeSet id="create-tnr-tables" author="you">
    <createTable tableName="tnr">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true"/>
        </column>
        <column name="name" type="VARCHAR(255)"/>
    </createTable>

    <createTable tableName="tnr_ids">
        <column name="id" type="BIGINT" autoIncrement="true">
            <constraints primaryKey="true"/>
        </column>
        <column name="tnr_id" type="BIGINT"/>
        <column name="value" type="BIGINT"/>
    </createTable>

    <addForeignKeyConstraint baseTableName="tnr_ids"
                             baseColumnNames="tnr_id"
                             referencedTableName="tnr"
                             referencedColumnNames="id"
                             constraintName="fk_tnr_ids"/>
</changeSet>
