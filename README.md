<div className={sidebarStyles.sidebar}>
    <h3 className={sidebarStyles.sidebarTitle}>Forms</h3>
    <ul className={sidebarStyles.formList}>
        <li onClick={() => setSelectedForm('PaymentInstructionForm')} className={sidebarStyles.formItem}>
            Payment Instruction Form
        </li>
        <li onClick={() => setSelectedForm('PaymentDetails')} className={sidebarStyles.formItem}>
            Payment Details
        </li>
        <li onClick={() => setSelectedForm('LostPolicyForm')} className={sidebarStyles.formItem}>
            Lost Policy Form
        </li>
        <li onClick={() => setSelectedForm('WitnessDetails')} className={sidebarStyles.formItem}>
            Witness Details
        </li>
    </ul>
</div>
