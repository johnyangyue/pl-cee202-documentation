Class Preparation
=====


Team Assignment
------------

To generate the team assignment file, we can use the script in the ``cee202_team_assignment.ipynb`` notebook.

The script requires us to obtain a list of students, which we can download from the Canvas gradebook and save it as ``roster.csv``.

Users can adjust ``roster = pd.read_csv('roster.csv')`` and ``size=4`` to according to the location ``roster.csv`` is stored and desired
average team size. 

.. code-block:: python
   :linenos:
   :emphasize-lines: 3,5-6
   # Use Canvas gradebook as roster.
   roster = pd.read_csv('roster.csv')

   # Team size (can be adjusted)
   size = 4

After removing un-necessary information and entries with 

.. code-block:: python
   :linenos:
   :emphasize-lines: 3,5-6
   roster = roster[roster.Role!='Staff'].reset_index(drop=True)
   roster['netid'] = roster['UID'].apply(lambda x:x.split('@')[0])
   roster = roster.rename(columns={'Name':'name'})

we generate a mapping from row indices of ``roster`` to team assignment with 

.. code-block:: python
   :linenos:
   :emphasize-lines: 3,5-6

   # Mapping from row index to team assignment
   team = {}
   random.seed(100)
   available_ids = [i for i in range(roster.shape[0])]

   for i in range(roster.shape[0]//size):
      
      # Sample size number of row indices from pool 
      sampled_ids = random.sample(available_ids,size)

      for j in range(size):
         # Assign the same group number of each sampled row index
         team[sampled_ids[j]] = i+1

         # Remove assigned row indices from pool
         available_ids.remove(sampled_ids[j])

   # Place unassigned rows to difference teams
   for i,id in enumerate(available_ids):
      team[id] = i+1

The final output is exported as ``CEE202_FindYourTeam.csv``, which we can upload to Canvas during the first week.


Canvas Maintenance
----------------



