 <Select
                      options={claimCategoryOptions}
                      value={claimCategoryOptions.find(option => option.value === claimCategoryvalue)}
                      onChange={(selectedOption) => handleOptionChangeForDropdownforclaimcategory(selectedOption.value)}
                      placeholder="--Select--"
                      styles={{ container: (provided) => ({ ...provided, width: 200 }) }}
                    />
